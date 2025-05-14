
![20250508_0555_Smart Lab Connectivity_simple_compose_01jtqzb97rfrh8wjcsh8rgjapt](https://github.com/user-attachments/assets/60f3d122-f2be-4707-8a6c-07d8d1e98dab)


# Monitoreo Ambiental Inteligente con Pico W y MongoDB

## Introducción

En esta práctica final de semestre desarrollaremos un sistema de **monitoreo ambiental inteligente** utilizando una **Raspberry Pi Pico W** como dispositivo IoT, un servidor **API REST con Flask** desplegado en una instancia **AWS EC2**, y una base de datos **MongoDB** para almacenar los datos. El objetivo del proyecto es que el microcontrolador **Pico W** mida la temperatura ambiente y envíe periódicamente esos datos a la nube, donde un servidor web los recibe y guarda de forma centralizada.

Esta guía paso a paso cubre la arquitectura general del sistema, el código necesario tanto en el Pico W (MicroPython) como en el servidor (Python Flask), la implementación de una autenticación sencilla mediante token en las peticiones HTTP, la estructura de los datos almacenados en MongoDB, pruebas manuales con herramientas como Postman/cURL, y cómo configurar el servidor para que el servicio Flask se ejecute automáticamente como un **demonio (servicio) de systemd**. Al final, se incluye la estructura de archivos del proyecto, un archivo `requirements.txt` con dependencias, y las instrucciones de instalación para poner todo en marcha.

## Arquitectura del Sistema

El sistema se compone de varios elementos que trabajan juntos de forma integrada. A continuación se describen los componentes principales y cómo interactúan entre sí:

* **Raspberry Pi Pico W (Microcontrolador)** – Ejecuta firmware MicroPython y está conectada a un sensor de temperatura (por ejemplo, un **DHT11**). La Pico W se conecta a una red Wi-Fi (en este caso una red abierta, sin contraseña) y envía los datos de temperatura mediante solicitudes HTTP POST a la API en la nube.

* **Sensor de Temperatura (DHT11)** – Sensor sencillo que mide la temperatura ambiente (y humedad, si se deseara). Está conectado a un pin de la Pico W. La Pico lee la temperatura del sensor periódicamente (por ejemplo, cada 10 segundos) para monitorear el ambiente.

* **Conexión Wi-Fi** – La Pico W se conecta a una red Wi-Fi local abierta, obteniendo acceso a Internet. Esto le permite comunicarse con el servidor remoto. *(Nota: En un entorno real se recomendaría usar una red Wi-Fi segura con contraseña, pero usamos una abierta para simplificar la configuración de la práctica.)*

* **Servidor Flask (API REST en EC2)** – Una aplicación web ligera escrita en Python usando el framework Flask. Está alojada en una instancia EC2 (servidor virtual en AWS) con sistema operativo Linux. El servidor expone un endpoint HTTP (por ejemplo, `/datos`) que recibe las mediciones de la Pico W en formato JSON vía POST. Este servidor se encargará de validar una **cabecera de autenticación (token)** en cada petición para asegurar que solo dispositivos autorizados envíen datos.

* **Base de Datos MongoDB** – Un motor de base de datos NoSQL orientado a documentos, instalado en la misma instancia EC2. El servidor Flask inserta en MongoDB cada lectura recibida, almacenando datos como la temperatura, el identificador del dispositivo y la marca de tiempo de la medición. MongoDB permite luego consultar o procesar estos datos de manera eficiente. *(En nuestro caso, MongoDB corre localmente en EC2; en una arquitectura de producción podría ser un servicio gestionado o una instancia separada.)*

**Flujo de datos:** La Pico W obtiene una medición de temperatura del sensor y la envía en una solicitud HTTP al endpoint del servidor Flask, incluyendo en la cabecera un token secreto para autenticación. El servidor Flask, al recibir la petición, verifica el token, procesa el JSON con los datos y los almacena en la base de datos MongoDB. Opcionalmente, podría responder con un mensaje de éxito. Este ciclo se repite periódicamente, logrando un monitoreo en tiempo real. La siguiente ilustración resume la arquitectura:


<img width="1463" alt="Screenshot 2025-05-07 at 5 31 42 p m" src="https://github.com/user-attachments/assets/ed4796d3-23a5-4663-94cc-74704fc1bd90" />



*Nota:* En esta práctica nos enfocamos en el envío y almacenamiento de datos. La visualización o análisis de los datos almacenados no se implementa aquí, pero la información quedará disponible en MongoDB para consultas futuras. Asimismo, aunque usamos HTTP sobre una red abierta para simplificar, en un sistema real se debería usar **HTTPS** para cifrar la comunicación y proteger tanto el token de autenticación como los datos transmitidos.

# Atajos y Acordeones de uso de MongoDB cliente

https://www.mongodb.com/developer/products/mongodb/cheat-sheet/



## Código del Pico W en MicroPython

En el microcontrolador Raspberry Pi Pico W programaremos un script en MicroPython que realice las siguientes tareas:

1. **Conexión a Wi-Fi:** activar el modo estación Wi-Fi de la Pico W, conectarse a la red inalámbrica abierta (SSID conocido, sin contraseña) y esperar hasta obtener conexión (IP asignada).
2. **Lectura de temperatura del sensor:** inicializar el sensor DHT11 y leer la temperatura en grados Celsius. *(Si se tuviera un sensor diferente o múltiples variables a medir, se podrían obtener aquí también.)*
3. **Envío de datos al servidor:** formatear los datos en JSON e incluir el token de autenticación en una cabecera HTTP, luego realizar una petición HTTP POST al endpoint de la API Flask con los datos de temperatura.
4. **Repetición periódica:** esperar unos segundos y volver a medir y enviar, continuamente, para un monitoreo constante.

---
Thought for a few seconds


# Monitoreo Ambiental Inteligente con Pico W y MongoDB

## Contexto y Enfoque para Estudiantes de Bases de Datos

Esta práctica introduce a los alumnos de **Taller de Bases de Datos** —quienes provienen de modelos relacionales clásicos (MySQL, PostgreSQL) y de bases de datos de series temporales como InfluxDB— al mundo de las **arquitecturas NoSQL**, con MongoDB como ejemplo principal.

* **MongoDB vs. Relacional**
  • Modelo de documentos JSON dinámicos vs. tablas con esquema fijo.
  • Flexibilidad para agregar campos sin alterar la colección.
  • Escalabilidad horizontal (sharding) nativa.

* **MongoDB vs. InfluxDB**
  • InfluxDB optimizada estrictamente para series temporales; MongoDB es más general pero soporta series temporales.
  • Permite almacenar metadatos y distintos tipos de medida en un mismo documento.

* **Importancia de NoSQL**
  • Desarrollo más rápido y flexible.
  • Escalabilidad para cargas variables de lectura/escritura.
  • Integración sencilla con aplicaciones web y arquitecturas IoT.

---

## 1. Estructura de Carpetas

```
monitoreo_proyecto/
├── pico/                      
│   └── main.py                # Firmware MicroPython para Pico W
└── servidor/
    ├── app.py                 # API Flask
    ├── requirements.txt       # Dependencias Python
    └── monitoreo.service      # Unit file systemd
```

> En la instancia EC2, copiar `monitoreo.service` a `/etc/systemd/system/monitoreo.service`.

---

## 2. Preparar el Servidor EC2

### 2.1 Actualizar Paquetería

```bash
sudo apt-get update
sudo apt-get upgrade -y
```

### 2.2 Instalar Dependencias

```bash
sudo apt-get install -y python3 python3-pip python3-venv 
# Iniciar y habilitar MongoDB

# REVISAR LA INSTALACION OFICIAL EN: https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

# Datos de prueva de restaurantes, AirBNB, peliculas via
https://www.mongodb.com/docs/atlas/sample-data/#std-label-available-sample-datasets


### 2.3 Desplegar Código y Entorno Virtual

```bash
cd /home/ubuntu
git clone <repo-url> monitoreo_proyecto
cd monitoreo_proyecto/servidor
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## 3. Firmware MicroPython (`pico/main.py`)

> **Requisitos para Pico W**
> • Actualiza el firmware de MicroPython desde [https://micropython.org/download/rp2-pico-w/](https://micropython.org/download/rp2-pico-w/)
> • Usa un IDE como **Mu Editor**, **Thonny** o **VS Code con la extensión Pico** para cargar `main.py`.

```python
import network, time, urequests, ujson, dht, machine

# ——— Configuración Wi-Fi ———
SSID     = "TecNM-ITT"      # Nombre de la red abierta
PASSWORD = ""               # Cadena vacía → red sin contraseña

# Conexión a Wi-Fi
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(SSID, PASSWORD)
print("Conectando a Wi-Fi…")
while not wlan.isconnected():
    time.sleep(1)
print("Conectado. IP:", wlan.ifconfig()[0])

# ——— Sensor DHT11 ———
sensor = dht.DHT11(machine.Pin(4))  # Pin GP4
print("Sensor DHT11 listo en GP4")

# ——— Datos del servidor ———
SERVER_URL = "http://<IP_SERVIDOR>:5000/api/datos"  # Reemplaza <IP_SERVIDOR>
API_TOKEN  = "TuTokenSecreto123"

def leer_temperatura():
    """Lee temperatura (°C) del DHT11."""
    sensor.measure()
    return sensor.temperature()

def enviar_lectura():
    """Envía JSON con temperatura al servidor Flask."""
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
