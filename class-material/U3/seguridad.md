🔐  El ejemplo actual **no está habilitada ninguna seguridad por token o clave de autenticación**, lo cual **deja la API completamente abierta**, es decir:

* Cualquiera que conozca la IP pública y el puerto `5000` puede enviar datos falsos.
* No hay validación de origen, identidad, ni integridad del dato.

---

### ✅ Solución rápida: Seguridad por **Token simple** en encabezado HTTP

Vamos a proteger la API para que **solo los dispositivos Pico W con un token autorizado puedan enviar datos**.

---

## 🛠️ Cambios en `server.py` (Flask)

Agrega este token al inicio del archivo:

```python
API_TOKEN = "TecNM2025_SECRET"  # Puedes cambiarlo por uno más seguro
```

Luego modifica el endpoint `/api/datos` así:

```python
@app.route('/api/datos', methods=['POST'])
def recibir_datos():
    token = request.headers.get('X-API-KEY')
    if token != API_TOKEN:
        return jsonify({"error": "Token inválido o ausente"}), 401

    data = request.get_json()
    data['timestamp'] = datetime.utcnow()
    resultado = coleccion.insert_one(data)
    return jsonify({"status": "ok", "id": str(resultado.inserted_id)})
```

---

## 🔐 En el Pico W (MicroPython)

Agrega el encabezado en la solicitud:

```python
headers = {"Content-Type": "application/json", "X-API-KEY": "TecNM2025_SECRET"}
respuesta = urequests.post("http://999.999.999.999:5000/api/datos", json=datos, headers=headers)
```

---

### 🔎 Resultado de este ejercicio "académico"

| Nivel de Seguridad | Descripción                                              |
| ------------------ | -------------------------------------------------------- |
| 🔓 Bajo (actual)   | Sin token, acepta cualquier solicitud                    |
| ✅ Medio            | Con token en encabezado HTTP (suficiente para educación) |
| 🔐 Alto            | Autenticación OAuth2/JWT + HTTPS + control de IPs        |

---

