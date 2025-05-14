
![20250508_0555_Smart Lab Connectivity_simple_compose_01jtqzb97rfrh8wjcsh8rgjapt](https://github.com/user-attachments/assets/60f3d122-f2be-4707-8a6c-07d8d1e98dab)


# Monitoreo Ambiental Inteligente con Pico W y MongoDB

## Introducción

En esta práctica final de semestre desarrollaremos un sistema de **monitoreo ambiental inteligente** utilizando una **Raspberry Pi Pico W** como dispositivo IoT, un servidor **API REST con Flask** desplegado en una instancia **AWS EC2**, y una base de datos **MongoDB** para almacenar los datos. El objetivo del proyecto es que el microcontrolador **Pico W** mida la temperatura ambiente y envíe periódicamente esos datos a la nube, donde un servidor web los recibe y guarda de forma centralizada.

Esta guía paso a paso cubre la arquitectura general del sistema, el código necesario tanto en el Pico W (MicroPython) como en el servidor (Python Flask), la implementación de una autenticación sencilla mediante token en las peticiones HTTP, la estructura de los datos almacenados en MongoDB, pruebas manuales con herramientas como Postman/cURL, y cómo configurar el servidor para que el servicio Flask se ejecute automáticamente como un **demonio (servicio) de systemd**. Al final, se incluye la estructura de archivos del proyecto, un archivo `requirements.txt` con dependencias, y las instrucciones de instalación para poner todo en marcha.

---

## Arquitectura del Sistema

**Componentes:**

* **Pico W:** Ejecuta MicroPython, conectado a la red Wi-Fi y lee temperatura desde el sensor interno.
* **Sensor Interno de Temperatura:** Utiliza el canal ADC4 del chip.
* **Wi-Fi:** Conexión a red abierta. 🔐 *Nota: No recomendado en producción.*
* **Servidor Flask:** Corriendo en puerto 5000 para recibir datos. ⚠️ *En producción usar Nginx en puerto 80 o 443.*
* **MongoDB:** Base de datos local NoSQL para almacenar documentos JSON.

**Flujo de datos:** Pico W → HTTP POST → Flask API → MongoDB

---

## Contexto Académico: Taller de Bases de Datos

### MongoDB vs Relacional

* Documentos JSON dinámicos vs tablas con esquema fijo
* Flexibilidad y escalabilidad horizontal

### MongoDB vs InfluxDB

* MongoDB es más general, permite metadatos diversos
* Soporte para series temporales (sin ser exclusivo)

### Ventajas NoSQL

* Desarrollo ágil
* Escalabilidad
* Integración sencilla con apps IoT

---

## Estructura del Proyecto

```
monitoreo_proyecto/
├── pico/                      
│   └── main.py                # Firmware MicroPython para Pico W
└── servidor/
    ├── app.py                 # API Flask
    ├── requirements.txt       # Dependencias Python
    └── monitoreo.service      # Unit file systemd
```

---

## Preparación del Servidor EC2

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y python3 python3-pip python3-venv

# Instalar MongoDB
# Ver: https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

---

## Despliegue del Proyecto

```bash
cd /home/ubuntu
git clone <repo-url> monitoreo_proyecto
cd monitoreo_proyecto/servidor
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## Código MicroPython en Pico W (`pico/main.py`)

> Requiere MicroPython actualizado y un editor como Mu, Thonny o VS Code

```python
import network, time, urequests, ujson, machine

SSID     = "TecNM-ITT"
PASSWORD = ""

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(SSID, PASSWORD)
print("Conectando a Wi-Fi…")
while not wlan.isconnected():
    time.sleep(1)
print("Conectado. IP:", wlan.ifconfig()[0])

sensor_temp = machine.ADC(4)
conversion_factor = 3.3 / 65535

SERVER_URL = "http://<IP_SERVIDOR>:5000/api/datos"
API_TOKEN  = "TuTokenSecreto123"

def leer_temperatura():
    lectura = sensor_temp.read_u16() * conversion_factor
    temperatura_c = 27 - (lectura - 0.706) / 0.001721
    return round(temperatura_c, 2)

def enviar_lectura():
    temp = leer_temperatura()
    payload = ujson.dumps({
        "dispositivo": "pico-w-1",
        "temperatura":  temp
    })
    headers = {
        "Content-Type":  "application/json",
        "X-API-Key":      API_TOKEN
    }
    try:
        resp = urequests.post(SERVER_URL, data=payload, headers=headers)
        if resp.status_code == 200:
            print("✓ Enviado:", temp, "°C")
        else:
            print("✗ Error:", resp.status_code)
        resp.close()
    except Exception as e:
        print("⚠️ Excepción:", e)

while True:
    enviar_lectura()
    time.sleep(10)
```

> 📦 Asegúrate de subir `urequests.py` y `ujson.py` si el firmware de MicroPython no las incluye.

---

## Código del Servidor Flask (`servidor/app.py`)

```python
from flask import Flask, request, jsonify
from datetime import datetime
from pymongo import MongoClient

app = Flask(__name__)

client = MongoClient("mongodb://localhost:27017/")
db = client["monitoreo_db"]
coleccion = db["mediciones"]

API_TOKEN = "TuTokenSecreto123"

@app.route('/api/datos', methods=['POST'])
def recibir_datos():
    token = request.headers.get("X-API-Key")
    if token != API_TOKEN:
        return jsonify({"error": "No autorizado"}), 401

    datos = request.get_json(silent=True)
    if not datos or "temperatura" not in datos:
        return jsonify({"error": "Formato inválido"}), 400

    datos["timestamp"] = datetime.utcnow().isoformat()
    try:
        coleccion.insert_one(datos)
    except Exception as e:
        return jsonify({"error": f"DB error: {e}"}), 500

    return jsonify({"status": "ok"}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

## Archivo `requirements.txt`

```
Flask==2.2.5
pymongo==4.3.3
```

---

## Servicio systemd (`servidor/monitoreo.service`)

```ini
[Unit]
Description=Monitoreo Ambiental Flask API
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/monitoreo_proyecto/servidor
ExecStart=/home/ubuntu/monitoreo_proyecto/servidor/venv/bin/python app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo cp servidor/monitoreo.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable monitoreo.service
sudo systemctl start monitoreo.service
```

---

## Prueba Manual con `curl`

```bash
curl -X POST http://<IP_SERVIDOR>:5000/api/datos \
     -H "Content-Type: application/json" \
     -H "X-API-Key: TuTokenSecreto123" \
     -d '{"dispositivo":"test-pc","temperatura":27.8}'
```

Esperado:

```json
{ "status": "ok" }
```

---

## Referencias Útiles

* Instalación MongoDB: [https://www.mongodb.com/docs/manual/](https://www.mongodb.com/docs/manual/)
* Cheat Sheet MongoDB: [https://www.mongodb.com/developer/products/mongodb/cheat-sheet/](https://www.mongodb.com/developer/products/mongodb/cheat-sheet/)
* Dataset de prueba: [https://www.mongodb.com/docs/atlas/sample-data/](https://www.mongodb.com/docs/atlas/sample-data/)
* Firmware MicroPython Pico W: [https://micropython.org/download/rp2-pico-w/](https://micropython.org/download/rp2-pico-w/)

---

**🚀 Proyecto listo para ser desplegado y usado como práctica de cierre de semestre.**

---

**🔐 Seguridad adicional recomendada para producción:**

* Usar Wi-Fi con contraseña WPA2 o WPA3.
* Servir Flask detrás de Nginx o Apache con HTTPS.
* MongoDB con autenticación activada (`auth=true`).


# ——— Bucle principal ———
while True:
    enviar_lectura()
    time.sleep(10)

```

> **Nota:** Asegúrate de subir `dht.py`, `urequests.py` y `ujson.py` al sistema de archivos de la Pico W si no están incluidos.

---

## 4. API Flask en EC2 (`servidor/app.py`)

```python
from flask import Flask, request, jsonify
from datetime import datetime
from pymongo import MongoClient

app = Flask(__name__)

# ——— Configuración MongoDB ———
client     = MongoClient("mongodb://localhost:27017/")
db         = client["monitoreo_db"]
coleccion  = db["mediciones"]

# ——— Token de autenticación ———
API_TOKEN = "TuTokenSecreto123"

@app.route('/api/datos', methods=['POST'])
def recibir_datos():
    # 1. Verificar token
    token = request.headers.get("X-API-Key")
    if token != API_TOKEN:
        return jsonify({"error": "No autorizado"}), 401

    # 2. Parsear JSON
    datos = request.get_json(silent=True)
    if not datos or "temperatura" not in datos:
        return jsonify({"error": "Formato inválido"}), 400

    # 3. Añadir timestamp y guardar
    datos["timestamp"] = datetime.utcnow().isoformat()
    try:
        coleccion.insert_one(datos)
    except Exception as e:
        return jsonify({"error": f"DB error: {e}"}), 500

    return jsonify({"status": "ok"}), 200

if __name__ == '__main__':
    # Ejecutar en todas las interfaces, puerto 5000
    app.run(host='0.0.0.0', port=5000)
```

---

## 5. Dependencias Python (`servidor/requirements.txt`)

```
Flask==2.2.5
pymongo==4.3.3
```

> Instala con:
>
> ```bash
> cd servidor
> source venv/bin/activate
> pip install -r requirements.txt
> ```

---

## 6. Servicio systemd (`servidor/monitoreo.service`)

```ini
[Unit]
Description=Monitoreo Ambiental Flask API
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/monitoreo_proyecto/servidor
ExecStart=/usr/bin/python3 /home/ubuntu/monitoreo_proyecto/servidor/app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

> En EC2:
>
> ```bash
> sudo cp servidor/monitoreo.service /etc/systemd/system/
> sudo systemctl daemon-reload
> sudo systemctl enable monitoreo.service
> sudo systemctl start monitoreo.service
> sudo systemctl status monitoreo.service
> ```

---

## 7. Prueba Manual con cURL

```bash
curl -X POST http://<IP_SERVIDOR>:5000/api/datos \
     -H "Content-Type: application/json" \
     -H "X-API-Key: TuTokenSecreto123" \
     -d '{"dispositivo":"test-pc","temperatura":27.8}'
```

* **Respuesta esperada:**

  ```json
  { "status": "ok" }
  ```

---

## 8. Árbol Final de Carpetas

```
/home/ubuntu/monitoreo_proyecto/
├── pico/
│   └── main.py
└── servidor/
    ├── app.py
    ├── requirements.txt
    └── monitoreo.service
```

---

Con estas actualizaciones, los estudiantes sabrán cómo preparar el servidor, actualizar el firmware de la Pico W con un IDE adecuado, y desplegar todo el sistema de forma práctica y ordenada. ¡Listo para la práctica de cierre de semestre!
