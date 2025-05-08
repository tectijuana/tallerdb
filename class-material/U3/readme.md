# Practicas de Mayo

# Raspberry Pico W o 2W o Wokwi(simulador)

🧩 **Arquitectura General del Proyecto**

1. **Pico W (MicroPython)**

   * Sensor simulado o real (temperatura, acelerómetro, etc.)
   * Envío de datos vía Wi-Fi (HTTP o MQTT)
   * Cliente HTTPS en MicroPython usando `urequests` o `umqtt.simple`

2. **Servidor EC2 con MongoDB (Ubuntu 22.04 recomendado)**

   * Instalación y configuración segura de MongoDB (puerto, IP binding, autenticación)
   * API intermediaria con Flask/FastAPI en Python que reciba los datos del Pico W
   * Conexión Python ↔ MongoDB usando `pymongo`

3. **Red (Seguridad y Acceso)**

   * Configuración de reglas de seguridad en AWS (Security Groups)
   * Token de autenticación o encabezado para proteger tu API

---

📦 **Ejemplo de Flujo de Datos**

<img width="1463" alt="Screenshot 2025-05-07 at 5 31 42 p m" src="https://github.com/user-attachments/assets/3bf1edca-95cf-49e7-8b0d-098dbdee326e" />

---

⚙️ **Práctica para estudiantes: "Monitoreo Ambiental Inteligente con Pico W y MongoDB"**

* 🧠 *Objetivo:* Enviar cada 10 segundos la temperatura del entorno desde un Pico W a un servidor MongoDB.
* 📚 *MicroPython:* Pico W genera datos y los manda en formato JSON por HTTP.
* 🖥️ *MongoDB:* EC2 almacena cada lectura como documento en una colección.
* 📊 *Extensión:* Consultar y graficar datos con Python desde MongoDB.

---

---

````markdown
# 🌡️ Proyecto IoT: Pico W + Flask + MongoDB

## 📶 Conexión del Pico W a Wi-Fi y Envío de Temperatura

### Código MicroPython para Raspberry Pi Pico W

```python
import network
import urequests
import time
import machine

# Wi-Fi abierta
ssid = "TecNM-ITT"
password = ""

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(ssid, password)

print("Conectando a Wi-Fi...")
while not wlan.isconnected():
    time.sleep(1)
print("Conectado a:", ssid)
print("IP:", wlan.ifconfig())

# Sensor interno de temperatura
sensor_temp = machine.ADC(4)
conversion_factor = 3.3 / 65535

def leer_temperatura():
    lectura = sensor_temp.read_u16() * conversion_factor
    temperatura = 27 - (lectura - 0.706) / 0.001721
    return round(temperatura, 2)

def enviar_datos():
    temperatura = leer_temperatura()
    datos = {
        "dispositivo": "PicoW_01",
        "sensor": "temperatura_interna",
        "valor": temperatura,
        "unidad": "°C"
    }
    try:
        respuesta = urequests.post("http://999.999.999.999:5000/api/datos", json=datos)
        print("Respuesta:", respuesta.text)
        respuesta.close()
    except Exception as e:
        print("Error:", e)

while True:
    enviar_datos()
    time.sleep(10)
````

---

## 🧠 Código del Servidor Flask en EC2

### Instalación de dependencias

```bash
sudo apt update
sudo apt install python3-pip -y
pip3 install flask pymongo flask-cors
```

### Código `server.py`

```python
from flask import Flask, request, jsonify
from flask_cors import CORS
from pymongo import MongoClient
from datetime import datetime

app = Flask(__name__)
CORS(app)

client = MongoClient("mongodb://localhost:27017/")
db = client.iot_pico
coleccion = db.lecturas_iot

@app.route('/api/datos', methods=['POST'])
def recibir_datos():
    data = request.get_json()
    data['timestamp'] = datetime.utcnow()
    resultado = coleccion.insert_one(data)
    return jsonify({"status": "ok", "id": str(resultado.inserted_id)})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

## 📊 Estructura del Registro en MongoDB

```json
{
  "_id": ObjectId("..."),
  "dispositivo": "PicoW_01",
  "sensor": "temperatura_interna",
  "valor": 29.43,
  "unidad": "°C",
  "timestamp": "2025-05-07T17:30:00Z"
}
```

---

## 📋 Vista de datos para estudiantes

| 🌐 Dispositivo | 🔍 Sensor            | 🌡️ Valor | 🕒 Timestamp            |
| -------------- | -------------------- | --------- | ----------------------- |
| PicoW\_01      | temperatura\_interna | 29.43°C   | 2025-05-07 17:30:00 UTC |
| PicoW\_01      | temperatura\_interna | 29.51°C   | 2025-05-07 17:30:10 UTC |

---

## 🧪 Prueba manual con Postman

```http
POST http://<EC2_PUBLIC_IP>:5000/api/datos
Content-Type: application/json

{
  "dispositivo": "PicoW_01",
  "sensor": "temperatura_interna",
  "valor": 29.43,
  "unidad": "°C"
}
```

---
# 📁 Código para el archivo flask_api.service

## Crea este archivo en /etc/systemd/system/flask_api.service:

```bash
mongosh
use iot_pico
db.lecturas_iot.find().pretty()
```


📁 Código para el archivo flask_api.service

```bash
[Unit]
Description=Flask API para Pico W y MongoDB
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/flask_api
ExecStart=/usr/bin/python3 /home/ubuntu/flask_api/server.py
Restart=always

[Install]
WantedBy=multi-user.target
```

⚠️ Asegúrate de que el script esté en /home/ubuntu/flask_api/server.py o cambia la ruta en ExecStart.

---

En Resumen:
Claro, aquí te muestro cómo debería verse el árbol de directorios del proyecto completo en tu servidor EC2 para que todo funcione correctamente:

```bash
/home/ubuntu/flask_api/
├── server.py                # Código del servidor Flask
├── requirements.txt         # Lista de dependencias Python
├── flask_api.service        # Archivo .service opcional (copia de referencia)
└── README.md                # Documentación del proyecto
```

Y el `.service` real debe estar en:

```bash
/etc/systemd/system/
└── flask_api.service        # Servicio de systemd que ejecuta Flask automáticamente
```

---

### 📄 Ejemplo de `requirements.txt`

Colócalo en `/home/ubuntu/flask_api/requirements.txt` para instalar dependencias fácilmente:

```txt
flask
pymongo
flask-cors
```

Instalas con:

```bash
cd /home/ubuntu/flask_api
pip3 install -r requirements.txt
```

---
