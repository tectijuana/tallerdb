

![20250508_0555_Smart Lab Connectivity_simple_compose_01jtqzb97rfrh8wjcsh8rgjapt](https://github.com/user-attachments/assets/7fe13072-f6b0-40c4-9be8-0edc9e153149)

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

```
[Sensor DHT11] --> [Pico W (MicroPython)] --(Wi-Fi, HTTP POST)--> [Flask API en EC2] --> [MongoDB en EC2]
```

*Nota:* En esta práctica nos enfocamos en el envío y almacenamiento de datos. La visualización o análisis de los datos almacenados no se implementa aquí, pero la información quedará disponible en MongoDB para consultas futuras. Asimismo, aunque usamos HTTP sobre una red abierta para simplificar, en un sistema real se debería usar **HTTPS** para cifrar la comunicación y proteger tanto el token de autenticación como los datos transmitidos.

## Código del Pico W en MicroPython

En el microcontrolador Raspberry Pi Pico W programaremos un script en MicroPython que realice las siguientes tareas:

1. **Conexión a Wi-Fi:** activar el modo estación Wi-Fi de la Pico W, conectarse a la red inalámbrica abierta (SSID conocido, sin contraseña) y esperar hasta obtener conexión (IP asignada).
2. **Lectura de temperatura del sensor:** inicializar el sensor DHT11 y leer la temperatura en grados Celsius. *(Si se tuviera un sensor diferente o múltiples variables a medir, se podrían obtener aquí también.)*
3. **Envío de datos al servidor:** formatear los datos en JSON e incluir el token de autenticación en una cabecera HTTP, luego realizar una petición HTTP POST al endpoint de la API Flask con los datos de temperatura.
4. **Repetición periódica:** esperar unos segundos y volver a medir y enviar, continuamente, para un monitoreo constante.

Asegúrate antes de comenzar que tu Pico W tiene el firmware de **MicroPython** instalado y está configurada en el entorno de desarrollo (por ejemplo, Thonny). También, se asume que cuentas con la librería `urequests` disponible en la Pico W para hacer solicitudes HTTP. (Si no la tienes, puedes obtener el módulo `urequests.py` del repositorio oficial de MicroPython y subirlo al sistema de archivos de la Pico.)

A continuación se presenta el código completo (`main.py`) para la Pico W, con comentarios explicativos en español:

```python
import network, time, urequests, ujson
import machine, dht

# Configuración de Wi-Fi
SSID = "NombreDeRedWiFi"      # SSID de la red Wi-Fi (sin contraseña en este caso)
PASSWORD = ""                # Contraseña de la red (vacía porque es una red abierta)
print("Conectando a la red Wi-Fi {}...".format(SSID))
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(SSID, PASSWORD)
# Esperar hasta estar conectado
while not wlan.isconnected():
    time.sleep(1)
print("Conectado a Wi-Fi. Dirección IP:", wlan.ifconfig()[0])

# Inicializar sensor DHT11 en el pin GP4 (pin físico 6 en la Pico W)
sensor = dht.DHT11(machine.Pin(4))
print("Sensor DHT11 inicializado en el pin GP4.")

# Configurar la URL del servidor Flask (reemplazar <IP_SERVIDOR> por la IP pública de tu EC2)
SERVER_URL = "http://<IP_SERVIDOR>:5000/datos"

# Token de autenticación (debe coincidir con el esperado por el servidor)
AUTH_TOKEN = "TuTokenSecreto123"

# Bucle principal: leer temperatura y enviar datos periódicamente
while True:
    try:
        # Medir la temperatura con el sensor
        sensor.measure()                      # Tomar lectura del DHT11
        temp_c = sensor.temperature()         # Temperatura en grados Celsius
        print("Temperatura medida:", temp_c, "°C")

        # Crear el payload de datos en formato JSON
        datos = {
            "dispositivo": "pico-w-1",       # identificador del dispositivo (nombre arbitrario)
            "temperatura": temp_c            # temperatura en °C
            # Podríamos añadir más campos como 'humedad': sensor.humidity() si se desea
        }
        json_data = ujson.dumps(datos)

        # Cabeceras HTTP, incluyendo Content-Type y el token de autenticación
        headers = {
            "Content-Type": "application/json",
            "X-API-Key": AUTH_TOKEN          # Enviamos el token en una cabecera personalizada
        }

        # Enviar la solicitud HTTP POST al servidor con los datos
        respuesta = urequests.post(SERVER_URL, data=json_data, headers=headers)
        # Verificar respuesta del servidor
        if respuesta.status_code == 200:
            print("Datos enviados correctamente al servidor.")
        else:
            print("Error al enviar datos. Código de respuesta:", respuesta.status_code)
        respuesta.close()  # Cerrar la conexión HTTP

    except Exception as e:
        print("Ocurrió un error durante la medición o envío:", e)

    # Esperar 10 segundos antes de la siguiente lectura/envío
    time.sleep(10)
```

**Explicación del código:** En el bloque anterior, primero activamos la interfaz Wi-Fi de la Pico W y nos conectamos al SSID especificado (`NombreDeRedWiFi`). Dado que es una red abierta, la contraseña se deja como cadena vacía. El código espera en un bucle hasta que `wlan.isconnected()` devuelve `True`, indicando que se ha obtenido conexión (se imprime la IP asignada para confirmación).

Luego se inicializa el sensor DHT11 en el pin GP4 (GPIO número 4 de la Pico W). A través de `sensor.measure()` se realiza una lectura y con `sensor.temperature()` obtenemos la temperatura en grados Celsius. (Si tuviéramos un **DHT22** u otro sensor, la inicialización cambiaría ligeramente, pero el concepto es similar. El DHT11 proporciona temperatura entera; un DHT22 daría un decimal, pero para el caso no importa).

Definimos la URL del servidor Flask donde enviaremos los datos. Debes reemplazar `"<IP_SERVIDOR>"` con la dirección IP pública (o nombre de host) de tu instancia EC2 donde corre Flask. Asegúrate de incluir el puerto correcto (en este ejemplo usamos el puerto 5000) y la ruta `/datos` del endpoint que configuraremos en Flask.

También definimos un token de autenticación (`AUTH_TOKEN`), que aquí asignamos a `"TuTokenSecreto123"` como ejemplo. Este valor debe ser **el mismo** que el que el servidor Flask espera para validar las peticiones. Es esencial mantener este token en secreto; en un entorno real no debería estar hardcodeado en el código (se podría cargar de una configuración), pero para esta práctica está bien colocarlo directamente.

El código entra luego en un bucle `while True` infinito, que periódicamente (cada 10 segundos) hará lo siguiente:

* Invocar `sensor.measure()` para actualizar las lecturas del sensor.
* Extraer la temperatura medida `temp_c` en grados Celsius.
* Construir un diccionario `datos` con la información a enviar. En este caso incluimos un identificador de dispositivo (`"dispositivo": "pico-w-1"`) para saber quién envía el dato, y la `"temperatura"` medida. (Se podrían agregar más campos, por ejemplo `humedad` si quisiéramos enviar también ese valor).
* Convertir el diccionario a una cadena JSON usando `ujson.dumps`.
* Preparar las cabeceras HTTP, especificando `"Content-Type": "application/json"` para indicar que el cuerpo es JSON, y la cabecera personalizada `"X-API-Key"` con el token de autenticación.
* Usar `urequests.post()` para hacer una petición POST al servidor Flask con la URL y cabeceras definidas, y pasando `json_data` como cuerpo (`data=...`). El servidor responderá con un código HTTP; si es `200 OK`, imprimimos que se enviaron los datos correctamente. Si devuelve otro código (por ejemplo `401 Unauthorized` si el token no coincide, o `500` si hubo un error en el servidor), lo informamos también. Finalmente cerramos la respuesta (`respuesta.close()`) para liberar la conexión.
* Si ocurre alguna excepción durante este proceso (por ejemplo, falla el sensor o la conexión), la capturamos e imprimimos el error para depuración.
* Esperamos 10 segundos (`time.sleep(10)`) antes de iterar nuevamente y enviar la siguiente lectura.

Con este script corriendo en la Pico W, el microcontrolador estará enviando continuamente datos de temperatura al servidor remoto. Asegúrate de guardar este código como `main.py` en la Pico W (por ejemplo, usando Thonny y subiendo el archivo a la raíz del sistema de archivos de la Pico). De ese modo, el código se ejecutará automáticamente cada vez que la Pico W reinicie.

## Código del Servidor Flask en EC2

En el lado del servidor, crearemos una aplicación Flask que reciba los datos enviados por la Pico W y los guarde en MongoDB. El servidor deberá exponer un endpoint (ruta URL) que acepte peticiones HTTP POST. Además, incorporaremos la verificación del token de autenticación en cada solicitud recibida.

A continuación se muestra el código completo de la aplicación Flask (`app.py`), con comentarios en español para explicar cada parte:

```python
from flask import Flask, request, jsonify
from datetime import datetime
from pymongo import MongoClient

app = Flask(__name__)

# Conectar a la base de datos MongoDB (localmente en la instancia EC2)
cliente = MongoClient("mongodb://localhost:27017/")
db = cliente["monitoreo_db"]                 # Nombre de la base de datos
coleccion = db["mediciones"]                 # Nombre de la colección para datos de sensor

# Token de autenticación esperado (debe coincidir con el que envía la Pico W)
TOKEN_ESPERADO = "TuTokenSecreto123"

@app.route('/datos', methods=['POST'])
def recibir_datos():
    """Endpoint para recibir datos de sensores (método POST)"""
    # 1. Autenticación mediante token en la cabecera
    token = request.headers.get('X-API-Key')
    if token is None or token != TOKEN_ESPERADO:
        # Si no se envió token o no coincide, responder con 401 (No Autorizado)
        return jsonify({"error": "No autorizado"}), 401

    # 2. Obtener el JSON del cuerpo de la petición
    datos = request.get_json(force=True)
    if datos is None:
        # Si no hay JSON válido en la petición
        return jsonify({"error": "Datos no proporcionados"}), 400

    # 3. Añadir marca de tiempo (timestamp) a los datos
    datos["timestamp"] = datetime.utcnow().isoformat()
    # (Opcionalmente, podríamos validar campos específicos aquí, e.g. asegurar que 'temperatura' esté en datos)

    try:
        # 4. Insertar los datos en la colección de MongoDB
        resultado = coleccion.insert_one(datos)
        # Obtener el id asignado por MongoDB (no es estrictamente necesario imprimirlo)
        nuevo_id = str(resultado.inserted_id)
        print(f"Dato insertado en MongoDB con _id={nuevo_id}")
    except Exception as e:
        # Si ocurre algún error al guardar en la base de datos
        return jsonify({"error": f"Error al guardar en DB: {e}"}), 500

    # 5. Responder con éxito
    return jsonify({"status": "ok"}), 200

if __name__ == '__main__':
    # Ejecutar la app Flask en el puerto 5000, accesible externamente
    app.run(host='0.0.0.0', port=5000)
```

**Explicación del servidor Flask:** Empezamos importando las librerías necesarias. `flask` proporcionará la clase Flask y utilidades para manejar peticiones (`request`) y generar respuestas JSON (`jsonify`). Importamos `datetime` para sellar las lecturas con fecha y hora, y `MongoClient` de `pymongo` para conectarnos a MongoDB.

Configuramos la conexión a MongoDB suponiendo que MongoDB está instalado y corriendo localmente en el mismo servidor (localhost) en el puerto estándar 27017. Usamos `MongoClient("mongodb://localhost:27017/")` para conectarnos al *daemon* de Mongo. A continuación definimos la base de datos (llamada `"monitoreo_db"`) y la colección (tabla de documentos) llamada `"mediciones"`. **Nota:** MongoDB crea automáticamente la base de datos y la colección la primera vez que se insertan datos si estas no existen, por lo que no hace falta crearlas explícitamente antes.

Definimos una constante `TOKEN_ESPERADO` que contiene el token de autenticación que el servidor considera válido. Debe ser idéntico al que configuramos en la Pico W (`TuTokenSecreto123` en este ejemplo). En un escenario real, podríamos gestionar los tokens de forma más segura, pero para la práctica lo manejamos así de forma sencilla.

Luego definimos la ruta de la API con un decorador: `@app.route('/datos', methods=['POST'])`. Esto crea un endpoint accesible via URL `<IP>:5000/datos` que acepta únicamente el método POST. La función asociada `recibir_datos()` realiza los pasos siguientes cuando llega una petición:

1. **Autenticación:** Extrae el valor de la cabecera `'X-API-Key'` de la petición (`request.headers.get('X-API-Key')`). Si la cabecera no está presente o su valor no coincide con `TOKEN_ESPERADO`, entonces se devuelve una respuesta de error con código 401 **No Autorizado**. Esto asegura que solo nuestros dispositivos (que conocen el token secreto) puedan enviar datos.
   *Nota:* Aquí usamos una cabecera personalizada `X-API-Key` para el token, pero también se podría usar la cabecera estándar `Authorization` (por ejemplo con un token Bearer). El enfoque es similar: verificar que el token proporcionado coincida con el esperado.

2. **Parsing de datos:** Utiliza `request.get_json(force=True)` para obtener el contenido JSON del cuerpo de la solicitud HTTP. Marcamos `force=True` para intentar interpretar el cuerpo como JSON incluso si el encabezado `Content-Type` no estuviera establecido, pero en nuestro caso sí lo enviamos correctamente desde la Pico W. Si por alguna razón `datos` es `None` (no llegó JSON válido), respondemos con un error 400 **Bad Request** indicando que no se proporcionaron datos.

3. **Agregar timestamp:** Tomamos el momento actual (`datetime.utcnow()`) en UTC y lo convertimos a cadena con formato ISO (`isoformat()`), luego almacenamos eso en `datos["timestamp"]`. De este modo, cada registro tendrá una marca temporal de cuándo lo recibió el servidor. (Alternativamente podríamos guardar el objeto datetime directamente; PyMongo lo convertiría a un tipo de fecha de MongoDB. Para simplificar la visualización, usamos cadena de texto ISO8601).

   También podríamos realizar aquí validaciones adicionales, por ejemplo verificar que ciertos campos clave estén en `datos` (como asegurarse de que `'temperatura'` exista y sea un número). Esto depende de qué tan robusta queramos hacer la API. En esta guía básica, asumimos que la Pico W siempre envía el formato correcto.

4. **Guardar en MongoDB:** Intentamos insertar el diccionario `datos` en la colección mediante `coleccion.insert_one(datos)`. Si la inserción es exitosa, MongoDB asignará automáticamente un identificador único `_id` al documento. Imprimimos en consola del servidor el `_id` generado a modo de registro (esto es opcional, pero útil para depuración o seguimiento de inserciones). Si ocurre alguna excepción al insertar (por ejemplo, si la base de datos no está accesible), capturamos la excepción y devolvemos un error 500 **Internal Server Error** con detalles.

5. **Responder al cliente:** Si todo salió bien, devolvemos una respuesta JSON con `{"status": "ok"}` y código 200 **OK**. La Pico W puede ignorar o usar esta respuesta; en nuestro código MicroPython simplemente verificamos el código para saber si fue exitoso.

Finalmente, el bloque `if __name__ == '__main__':` se asegura de ejecutar la aplicación Flask directamente si el script se lanza manualmente. Usamos `app.run(host='0.0.0.0', port=5000)` para que el servidor escuche en todas las interfaces de red (necesario para aceptar conexiones externas en EC2) y en el puerto 5000. Asegúrate de que este puerto esté **habilitado en el firewall o Security Group de la instancia EC2**, ya que AWS por defecto bloquea puertos entrantes no autorizados. (En la configuración de AWS Security Groups, agrega una regla para permitir tráfico TCP entrante en el puerto 5000, limitado a la IP desde la que probarás o abierto si es una prueba pública.)

Con este código en funcionamiento en EC2, el servidor estará listo para recibir las lecturas de la Pico W de forma segura y almacenarlas en la base de datos.

## Autenticación mediante Token en la API (HTTP Headers)

Para reforzar la seguridad de nuestra API (aunque sea de forma básica), **agregamos autenticación por token** en los encabezados HTTP de las solicitudes. Ya hemos visto la implementación en el código tanto del cliente (Pico W) como del servidor Flask, pero profundicemos un poco en este mecanismo:

* **¿Qué es un token y por qué usarlo?** Un token de autenticación es una cadena secreta (como una contraseña) que el cliente incluye en cada petición, y que el servidor verifica antes de procesar los datos. De esta manera, si alguien desconocido descubre la URL de nuestro endpoint, no podrá enviar datos falsos a menos que conozca el token. Es una capa de seguridad sencilla útil para APIs privadas o proyectos académicos. (En sistemas profesionales se usarían métodos más robustos, pero este enfoque ilustrativo es suficiente aquí.)

* **En la Pico W (cliente IoT):** En el código MicroPython definimos una variable `AUTH_TOKEN` con el valor secreto. Al preparar la solicitud POST, añadimos una cabecera HTTP personalizada llamada `"X-API-Key"` con ese token: `headers = {"X-API-Key": AUTH_TOKEN, ...}`. Esto significa que cada petición llevará en sus encabezados algo como `X-API-Key: TuTokenSecreto123`.

* **En el Servidor Flask:** Antes de aceptar o guardar ningún dato, recuperamos la cabecera esperada con `request.headers.get('X-API-Key')`. Comparamos el token recibido con el valor que hemos configurado (`TOKEN_ESPERADO`). Si no coinciden (o la cabecera falta), inmediatamente se devuelve un error 401, rehusando el procesamiento. Solo si el token es correcto el flujo continúa para guardar la información.

Este esquema de token es estático (el mismo token hardcodeado en ambos lados). En un despliegue real, podríamos implementar tokens dinámicos (por ejemplo JWTs) o usar autenticación mediante API keys que se puedan revocar, pero para propósitos de la práctica el token fijo funciona. Es importante **no compartir** ni exponer públicamente el token; trátalo como una contraseña. En el repositorio o documentación, evita subir el valor real del token si el proyecto fuera público (aquí usamos uno de ejemplo).

En resumen, gracias a esta autenticación por token, nuestro endpoint `/datos` solo aceptará peticiones legítimas de nuestro dispositivo Pico W, y rechazará intentos no autorizados, añadiendo así un nivel de seguridad al monitoreo ambiental.

## Estructura de Datos en MongoDB

Cuando el servidor Flask inserta los datos en MongoDB, estos se almacenan como **documentos JSON** dentro de la colección `mediciones` de la base de datos `monitoreo_db`. Cada documento corresponde a una medición enviada por la Pico W. A continuación describimos la estructura esperada de estos datos y proporcionamos un ejemplo:

* **Campos almacenados por medición:**

  * `dispositivo`: Identificador del dispositivo que envió la lectura (por ejemplo `"pico-w-1"` en nuestro código de ejemplo). Esto es útil si en el futuro tuviésemos múltiples Pico W enviando datos al mismo servidor.
  * `temperatura`: Valor de la temperatura medida en grados Celsius (número, puede ser entero o decimal).
  * `timestamp`: Marca de tiempo en formato ISO 8601 indicando cuándo se recibió la lectura. En nuestro código, se genera en el servidor en UTC, por ejemplo `"2025-05-08T10:00:00.123456"` (ISO8601 con microsegundos) o similar. Esto nos permite saber la fecha y hora de cada medición.
  * `_id`: Identificador único de MongoDB para el documento (lo genera automáticamente Mongo al insertar). Es un valor de tipo ObjectId, por ejemplo `"645acfd39f1b4b23ac39d8e1"`. Este campo no lo enviamos nosotros; MongoDB lo agrega para cada documento insertado.

**Ejemplo de documento (medición) en MongoDB:** Supongamos que nuestra Pico W (dispositivo "pico-w-1") envía dos lecturas de temperatura. Los documentos en la colección podrían verse así (formato JSON simplificado):

```json
{
  "_id": ObjectId("645ad1f49f1b4b23ac39d8e2"),
  "dispositivo": "pico-w-1",
  "temperatura": 24.5,
  "timestamp": "2025-05-08T10:00:00.123456"
}
{
  "_id": ObjectId("645ad2a79f1b4b23ac39d8e3"),
  "dispositivo": "pico-w-1",
  "temperatura": 24.7,
  "timestamp": "2025-05-08T10:05:00.678912"
}
```

Para mayor claridad, a continuación presentamos una **tabla de ejemplo** con los campos principales de una medición y valores hipotéticos:

| `_id` (ObjectId)         | dispositivo | temperatura (°C) | timestamp                  |
| ------------------------ | ----------- | ---------------- | -------------------------- |
| 645ad1f49f1b4b23ac39d8e2 | pico-w-1    | 24.5             | 2025-05-08T10:00:00.123456 |
| 645ad2a79f1b4b23ac39d8e3 | pico-w-1    | 24.7             | 2025-05-08T10:05:00.678912 |

*Nota:* El campo `_id` es generado automáticamente por MongoDB y es único para cada documento insertado. Los valores de timestamp se muestran con formato de fecha y hora ISO; aquí incluimos microsegundos para ilustrar, pero el nivel de precisión puede variar. En muchos casos será suficiente con segundos, e incluso podríamos formatear el timestamp para quitar los microsegundos o añadir un indicativo de zona horaria (por simplicidad usamos UTC sin indicar zona, asumiendo UTC).

Con esta estructura de datos, los estudiantes pueden fácilmente consultar la colección `mediciones` usando, por ejemplo, la interfaz de MongoDB (como **MongoDB Compass** o la consola `mongo`) para ver las lecturas almacenadas. Cada fila de la tabla conceptual corresponde a un documento JSON en la base de datos.

**Consejo:** Puedes intentar hacer una consulta básica a la base de datos para comprobar los datos. Si estás en la instancia EC2, podrías usar la consola de MongoDB: por ejemplo, entrando al shell (`mongo`) y ejecutando `use monitoreo_db` y luego `db.mediciones.find().pretty()` para listar las mediciones en formato legible. Verás los documentos con los campos como los del ejemplo.

## Prueba Manual con Postman o cURL

Antes de integrar todo con la Pico W, es recomendable probar manualmente que el servidor Flask recibe y almacena datos correctamente. Podemos simular el envío de una lectura usando herramientas como **Postman** (interfaz gráfica para probar APIs) o **cURL** (comando de terminal para hacer peticiones HTTP). A continuación, describimos cómo realizar una prueba manual:

**Usando cURL (línea de comando):** Si tienes acceso a una terminal (Linux, macOS, o Windows con WSL/PowerShell), cURL permite enviar una petición POST con facilidad. Asegúrate de reemplazar `<IP_SERVIDOR>` y `<TOKEN>` con los valores correspondientes de tu configuración.

```bash
curl -X POST http://<IP_SERVIDOR>:5000/datos \
     -H "Content-Type: application/json" \
     -H "X-API-Key: TuTokenSecreto123" \
     -d '{"dispositivo": "prueba-manual", "temperatura": 30.5}'
```

Desglose del comando:

* `-X POST` indica que es una petición POST.
* La URL completa incluye el puerto 5000 y la ruta `/datos` de nuestro endpoint Flask.
* `-H` se usa para añadir encabezados HTTP. Aquí enviamos `Content-Type: application/json` (para que Flask sepa que el cuerpo es JSON) y `X-API-Key: TuTokenSecreto123` (el token de autenticación, igual al que espera el servidor).
* `-d '{"dispositivo": "prueba-manual", "temperatura": 30.5}'` especifica el cuerpo de la petición. En este caso enviamos un JSON con un campo `"dispositivo"` indicando, por ejemplo, "prueba-manual" (para distinguir que es una prueba desde nuestra PC y no de la Pico) y `"temperatura": 30.5` (un valor cualquiera de temperatura para la prueba).

Al ejecutar este comando, deberías recibir como respuesta un JSON con `{"status": "ok"}` si todo funcionó, y en la consola del servidor Flask verías un mensaje de inserción en MongoDB. También podrías comprobar en la base de datos que el registro fue agregado.

**Usando Postman (interfaz gráfica):**

1. Abre Postman y crea una **nueva petición** de tipo POST.
2. En la barra de URL, ingresa `http://<IP_SERVIDOR>:5000/datos` (nuevamente reemplazando `<IP_SERVIDOR>` por la IP pública de tu EC2).
3. En la pestaña "Headers" (Encabezados), agrega una clave `"Content-Type"` con valor `"application/json"`. Agrega otra clave `"X-API-Key"` con valor `"TuTokenSecreto123"`.
4. En la pestaña "Body", selecciona el tipo "raw" y el formato "JSON". Luego escribe un cuerpo JSON de prueba, por ejemplo:

   ```json
   {
       "dispositivo": "postman-test",
       "temperatura": 28.3
   }
   ```
5. Envía la petición ("Send").
6. Observa la respuesta en Postman. Debería mostrar un JSON `{"status": "ok"}` con código de estado 200 OK si la autenticación y el almacenamiento fueron exitosos. Si ves un error 401, revisa que hayas escrito correctamente el token en el encabezado. Si ves 500, podría ser un problema de conexión con la base de datos en el servidor.
7. (Opcional) Verifica en tu base de datos MongoDB que el documento con `"dispositivo": "postman-test"` ahora esté presente.

Esta prueba manual es muy útil para aislar problemas. Si algo falla aquí, debes corregirlo antes de intentar con la Pico W. Por ejemplo, puede revelar problemas como puerto bloqueado, token incorrecto, servidor Flask caído, etc. Una vez que Postman o cURL funcionen, puedes proceder a encender la Pico W con confianza de que sus peticiones serán atendidas correctamente.

## Creación de un Servicio systemd para Flask

Para que el servidor Flask se ejecute de forma automática en la instancia EC2 (por ejemplo, que inicie al arrancar el sistema y se mantenga corriendo en segundo plano), es conveniente configurar un **servicio systemd**. Systemd es el sistema de inicialización y gestor de servicios en distribuciones Linux modernas (como Ubuntu, Amazon Linux, etc.), y nos permite definir un *unit file* `.service` que describa cómo iniciar nuestra aplicación.

A continuación, crearemos un archivo de servicio systemd llamado `monitoreo.service`. Estos son los pasos a seguir en la instancia EC2 (como usuario root o usando sudo):

1. **Crear el archivo de unidad**: Ejecuta `sudo nano /etc/systemd/system/monitoreo.service` para crear/editar el archivo de servicio (puedes usar otro editor si prefieres). Luego coloca el siguiente contenido en el archivo:

   ```ini
   [Unit]
   Description=API Monitoreo Ambiental Flask
   After=network.target

   [Service]
   User=ubuntu
   WorkingDirectory=/home/ubuntu/monitoreo
   Environment="PATH=/home/ubuntu/monitoreo/venv/bin"
   ExecStart=/home/ubuntu/monitoreo/venv/bin/python app.py
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

   **Explicación de este archivo**:

   * La sección `[Unit]` incluye una descripción y especifica que este servicio debe iniciarse después de que la red esté levantada (`After=network.target`), ya que nuestra app depende de la red (escucha en un puerto).
   * En `[Service]`, indicamos el usuario del sistema que ejecutará el servicio (aquí `User=ubuntu`, asumiendo que estamos usando el usuario ubuntu en EC2; ajusta esto si usas otro usuario).
     `WorkingDirectory` es la ruta del directorio de trabajo donde reside nuestro proyecto Flask (ajústalo a la ubicación real de tu código `app.py`; en este ejemplo asumimos `/home/ubuntu/monitoreo`).
     La variable de entorno `PATH` se establece para incluir el binario de Python de nuestro entorno virtual (venv) del proyecto, de manera que use las dependencias instaladas allí. (Si no usas virtualenv, podrías apuntar directamente a `/usr/bin/python3` y asegurarte de haber instalado las dependencias globalmente, aunque se recomienda usar un venv).
     `ExecStart` indica el comando para lanzar la app: aquí llamamos al intérprete Python de la venv para ejecutar `app.py`.
     `Restart=always` le dice a systemd que reinicie automáticamente el servicio si se detiene inesperadamente (por ejemplo, si el programa crashea).
   * La sección `[Install]` especifica que este servicio se quiere iniciar en el nivel multi-usuario (es decir, en el arranque normal del sistema).

   Guarda y cierra el archivo una vez editado.

2. **Recargar la configuración de systemd**: Como hemos agregado un nuevo unit file, ejecuta el comando:

   ```
   sudo systemctl daemon-reload
   ```

   Esto hace que systemd lea de nuevo los archivos de unidades para reconocer el nuevo servicio que creamos.

3. **Habilitar el servicio para inicio automático**: Ejecute:

   ```
   sudo systemctl enable monitoreo.service
   ```

   Esto marcará nuestro servicio para que se inicie en cada arranque del sistema. (Básicamente crea un enlace simbólico en `multi-user.target.wants`).

4. **Iniciar el servicio ahora mismo**: Puedes arrancar inmediatamente el servidor Flask mediante:

   ```
   sudo systemctl start monitoreo.service
   ```

   Este comando lanzará nuestra aplicación Flask en segundo plano.

5. **Verificar el estado**: Comprueba que esté corriendo correctamente con:

   ```
   sudo systemctl status monitoreo.service
   ```

   Deberías ver un estado "active (running)". El log mostrado debe incluir cualquier salida de nuestra aplicación. Puedes salir de la vista de estado presionando `q`.
   Si algo falló (estado "failed"), revisa los logs con `sudo journalctl -u monitoreo.service -xe` para ver mensajes de error. Problemas comunes podrían ser ruta incorrecta en WorkingDirectory/ExecStart, o alguna dependencia faltante.

Una vez completados estos pasos, tu servidor Flask estará ejecutándose como un servicio de sistema. Esto significa que si la instancia EC2 se reinicia, systemd levantará automáticamente el servicio de monitoreo sin intervención manual. También facilita su gestión (por ejemplo, puedes usar `sudo systemctl stop monitoreo.service` para detenerlo o `restart` para reiniciarlo tras algún cambio en el código).

**Nota:** Si actualizas el código de `app.py`, recuerda reiniciar el servicio para que los cambios tengan efecto. Y cada vez que modifiques el archivo `.service`, debes ejecutar de nuevo `daemon-reload`.

En este punto, tenemos la Pico W enviando datos periódicamente y un servidor robusto recibiendo y guardando la información continuamente en segundo plano.

## Estructura de Carpetas del Proyecto

Para mantener el proyecto organizado, es útil definir una estructura clara de archivos y directorios tanto para el código de la Pico W como para el servidor. A continuación se muestra un posible **árbol de carpetas** para este proyecto:

```
monitoreo_proyecto/
├── pico/                        # Código para la Raspberry Pi Pico W
│   └── main.py                 # Script MicroPython (mide temperatura y envía datos)
└── servidor/                    # Código para el servidor en EC2
    ├── app.py                  # Aplicación Flask (API REST)
    ├── requirements.txt        # Dependencias Python necesarias (Flask, PyMongo, etc.)
    └── monitoreo.service       # Archivo de servicio systemd para correr Flask como demonio
```

**Descripción**: El directorio `pico/` contiene el código que irá en la Pico W (en este caso solo un archivo `main.py`, que es el que se ejecutará automáticamente en el microcontrolador al iniciar). El directorio `servidor/` contiene la aplicación Flask (`app.py`), el archivo `requirements.txt` con las bibliotecas requeridas para que funcione, y el archivo de configuración del servicio `monitoreo.service`.

En un repositorio o entrega de la práctica, esta estructura ayuda a identificar fácilmente qué parte del código corre en el dispositivo embebido y qué parte corre en la nube. Recuerda que el archivo `monitoreo.service` realmente debe ser colocado en `/etc/systemd/system/` en el servidor, pero se incluye aquí para referencia y para mantenerlo bajo control de versiones si fuera el caso.

*(Si usas un entorno virtual para Python en el servidor, ese ambiente (por ejemplo una carpeta `venv/`) también existiría dentro de `servidor/`, pero usualmente no se incluye en el repositorio ni en la documentación. Lo importante es listar los requisitos en `requirements.txt`.)*

## Archivo requirements.txt e Instrucciones de Instalación

El archivo **`requirements.txt`** lista las dependencias de Python que necesitamos instalar en el servidor EC2 para que nuestro proyecto funcione. Para este proyecto, los paquetes necesarios son principalmente Flask y PyMongo. Asegúrate de tener versiones compatibles. Un ejemplo de `requirements.txt` sería:

```txt
Flask==2.2.5
pymongo==4.3.3
```

*(Puedes usar versiones específicas como arriba o simplemente los nombres de los paquetes. Incluir versiones exactas ayuda a reproducir el entorno, pero no es obligatorio si se está trabajando con las últimas versiones.)*

**Instrucciones de Instalación en EC2 (servidor):**

A continuación, se describen los pasos para configurar el entorno del servidor en una instancia Ubuntu EC2:

1. **Actualizar sistema e instalar MongoDB y Python**:

   * Conéctate a tu instancia EC2 vía SSH.
   * Actualiza la lista de paquetes e instala MongoDB, Python3 y pip (si no los tienes ya). Por ejemplo:

     ```bash
     sudo apt-get update
     sudo apt-get install -y mongodb python3 python3-pip python3-venv
     ```

     Esto instala MongoDB Community Edition desde los repositorios de Ubuntu (la versión puede variar, pero es suficiente para nuestra práctica), Python 3, pip para Python 3, y el módulo `venv` para crear entornos virtuales.
   * Inicia el servicio de MongoDB (si no se inicia solo) y habilítalo:

     ```bash
     sudo systemctl start mongodb
     sudo systemctl enable mongodb
     ```

     Verifica que MongoDB esté corriendo con `sudo systemctl status mongodb` (debería estar active). Por defecto, MongoDB escuchará en puerto 27017 localmente.

2. **Descargar o desplegar el código del servidor**:

   * Coloca el archivo `app.py` (nuestro código Flask) en algún directorio del servidor, por ejemplo en `/home/ubuntu/monitoreo/` (como asumimos en la configuración systemd).
   * También sube el archivo `requirements.txt` a ese directorio. Si estás usando Git, podrías clonar un repositorio; de lo contrario, puedes usar herramientas como `scp` o copiar-pegar el contenido vía SSH.

3. **Crear entorno virtual (opcional pero recomendado)**:

   * Navega al directorio del proyecto: `cd /home/ubuntu/monitoreo`.
   * Crea un entorno virtual de Python: `python3 -m venv venv`
   * Activa el entorno: `source venv/bin/activate`
   * (Tras activar, tu prompt de shell debería mostrar el nombre del venv, por ejemplo `(venv)`).

4. **Instalar dependencias Python**:

   * Con el entorno virtual activado (o omitido este paso si decides instalar globalmente), instala los requisitos:

     ```bash
     pip install -r requirements.txt
     ```
   * Esto descargará e instalará Flask, PyMongo y sus dependencias. Verifica que no hubo errores y que al final indica que se instalaron correctamente.

5. **Configurar variables (si aplica)**:

   * En nuestro caso, el token secreto y la configuración de la base de datos están embebidos en el código. No obstante, si quisieras, podrías configurar variables de entorno para ellos. Por simplicidad no lo hacemos aquí, pero ten en cuenta que en despliegues reales es preferible no hardcodear secretos en el código.
   * Si planeas cambiar el puerto de Flask o la ruta, ajusta el código antes de ejecutarlo.

6. **Probar la aplicación Flask manualmente** (opcional pero útil):

   * Aún sin configurar systemd, puedes probar ejecutar la app para depurar:

     ```bash
     python app.py
     ```
   * Deberías ver en la terminal un mensaje como *"Running on [http://0.0.0.0:5000/](http://0.0.0.0:5000/) (Press CTRL+C to quit)"*. Esto significa que el servidor está corriendo. Intenta hacer una prueba rápida con curl o Postman desde tu máquina local a la IP de EC2 para ver si responde (como se explicó en la sección de prueba manual). Si todo luce bien, detén la aplicación presionando `Ctrl+C` en el servidor para proceder a configurar el servicio.

7. **Configurar el servicio systemd**:

   * Crea el archivo `/etc/systemd/system/monitoreo.service` con el contenido proporcionado en la sección anterior **Creación de un Servicio systemd**. Asegúrate de que `WorkingDirectory` y las rutas apuntan correctamente a tu proyecto y entorno.
   * Recarga systemd y habilita/inicia el servicio como se indicó (commands `daemon-reload`, `enable`, `start`).
   * Comprueba que el servicio esté corriendo (`status`).

8. **Desplegar el código en la Pico W**:

   * Con el servidor listo, ahora asegúrate de que la Pico W tenga cargado el script MicroPython `main.py` (de la sección de código Pico W). Usa Thonny o tu IDE preferido para copiar el código al dispositivo. Recuerda poner la IP correcta del servidor y el mismo token en el código antes de grabarlo en la Pico.
   * Conecta la Pico W a una fuente de alimentación (puede ser el mismo USB de la PC). Deberías ver en el **Shell** de Thonny (o via UART serial) los prints del proceso de conexión Wi-Fi y envío de datos. Una vez que diga "Datos enviados correctamente al servidor.", puedes verificar en el servidor que efectivamente los datos llegaron (por ejemplo, mirando los logs del servicio Flask con `journalctl -u monitoreo.service -n 10` para las últimas líneas, o consultando la base de datos).

Si todos estos pasos se ejecutaron correctamente, habrás instalado y puesto en funcionamiento el sistema completo. En resumen, instalamos los componentes necesarios en EC2 (MongoDB, Python, dependencias), desplegamos la aplicación Flask y la convertimos en un servicio persistente, y cargamos el programa en la Pico W para que comience a enviar datos.

## Conclusión

Con la configuración anterior, hemos implementado un sistema de **IoT** funcional: la Raspberry Pi Pico W recoge datos de temperatura del entorno y los envía de forma inalámbrica a un servidor en la nube, donde se almacenan en una base de datos MongoDB. Este proyecto integra conocimientos de programación de microcontroladores (MicroPython), desarrollo de APIs web (Flask) y bases de datos NoSQL, reflejando un caso de uso real de **monitoreo remoto de sensores**.

Los estudiantes pueden expandir esta base añadiendo más tipos de sensores (por ejemplo, humedad, iluminación, calidad de aire), más nodos Pico W enviando datos, o creando una interfaz para visualizar las mediciones en tiempo real. También se podrían incorporar medidas de seguridad adicionales, como cifrar las comunicaciones con HTTPS o usar tokens dinámicos.

Esperamos que esta práctica final sirva para consolidar sus habilidades en sistemas embebidos conectados a la nube. ¡Con el sistema en marcha, ya están monitoreando el ambiente de forma inteligente y remota! Cada nuevo dato que aparece en MongoDB es una evidencia del flujo IoT funcionando correctamente. ¡Buen trabajo!
