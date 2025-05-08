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

Perfecto, aquí tienes el **código completo del servidor Flask** que recibiría los datos desde la Raspberry Pi Pico W y los insertará en una colección MongoDB. Este ejemplo está listo para correr en tu instancia EC2 (Ubuntu recomendado) en AWS.

---

### 📦 Requisitos (instalar en tu EC2 Ubuntu)

```bash
sudo apt update
sudo apt install python3-pip -y
pip3 install flask pymongo flask-cors
```

---

### 🧠 Código Flask API (`server.py`)

```python
from flask import Flask, request, jsonify
from flask_cors import CORS
from pymongo import MongoClient
from datetime import datetime

app = Flask(__name__)
CORS(app)

# Conexión a MongoDB (cambia localhost si tu Mongo está remoto)
client = MongoClient("mongodb://localhost:27017/")
db = client.iot_pico
coleccion = db.lecturas_iot

@app.route('/api/datos', methods=['POST'])
def recibir_datos():
    data = request.get_json()

    # Agregar timestamp actual
    data['timestamp'] = datetime.utcnow()

    # Insertar en MongoDB
    resultado = coleccion.insert_one(data)
    return jsonify({"status": "ok", "id": str(resultado.inserted_id)})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

### 🌐 Prueba desde navegador o Postman

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

### ✅ Verificación en MongoDB

Puedes usar `mongosh` para ver los registros guardados:

```bash
mongosh
use iot_pico
db.lecturas_iot.find().pretty()
```

---


¿Quieres que te prepare un ejemplo funcional de cada parte (código del Pico W, código del servidor Flask y configuración básica de MongoDB en EC2)?
