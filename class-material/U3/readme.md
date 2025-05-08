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

¿Quieres que te prepare un ejemplo funcional de cada parte (código del Pico W, código del servidor Flask y configuración básica de MongoDB en EC2)?
