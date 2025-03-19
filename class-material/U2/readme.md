

<img width="821" alt="Screenshot 2024-03-17 at 13 42 45" src="https://github.com/user-attachments/assets/f9db5bad-fcca-42da-86c0-8797d8042396" />

# Servicios LAMP

# Introducción a LAMP

LAMP es un acrónimo que representa una pila de tecnologías ampliamente utilizada para el desarrollo de aplicaciones web. El nombre LAMP proviene de los componentes que la forman:

- **Linux**: El sistema operativo base en el que se ejecutan todos los componentes.
- **Apache**: Un servidor web que gestiona las solicitudes HTTP y sirve contenido web.
- **MySQL**: Un sistema de gestión de bases de datos relacional que se usa para almacenar datos.
- **PHP**: Un lenguaje de programación del lado del servidor, utilizado para crear dinámicamente contenido web.

Esta combinación de tecnologías es muy popular debido a su bajo costo, robustez y flexibilidad. En un taller de bases de datos, aprenderemos a configurar, administrar y utilizar un servidor LAMP para desarrollar aplicaciones que interactúan con bases de datos MySQL, creando, gestionando y consultando datos eficientemente.


# Repositorio y perfil del Sr Teddy

https://github.com/teddysun  **Debe por el momento Ubuntu 22 LTS o DEBIAN ultima version**


----

![Screenshot 2025-03-11 at 9 45 53 p m](https://github.com/user-attachments/assets/58730816-c108-4f81-8c58-db4d414e3bd4)


# Mockaroo y su asistente Fabricate

Mockaroo es una herramienta en línea que permite generar datos simulados (fake data) para pruebas y desarrollo. Es muy útil cuando necesitas poblar una base de datos con datos ficticios pero realistas, sin necesidad de ingresarlos manualmente.

Características principales de Mockaroo:

- ✅ Generación de Datos Realistas: Puedes crear conjuntos de datos con nombres, direcciones, correos electrónicos, fechas, números de teléfono, entre otros.
- ✅ Formatos de Exportación: Permite descargar los datos en formatos como CSV, JSON, SQL, Excel y otros.
- ✅ Campos Personalizables: Puedes definir qué tipo de datos necesitas y generar registros masivos de forma automática.
- ✅ Integración con SQL: Puede generar sentencias INSERT para poblar bases de datos en MySQL, PostgreSQL, SQL Server, etc.
- ✅ API para Datos Dinámicos: Tiene una API para solicitar datos en tiempo real desde una aplicación.

Casos de uso
- 🔹 Poblar bases de datos para pruebas de rendimiento.
- 🔹 Crear datos de prueba para desarrollo sin comprometer información real.
- 🔹 Simular escenarios en análisis de datos o machine learning.
- 🔹 Generar datos de usuarios, productos, transacciones y más.

NOTA: por si solo mockaroo no valida datos, para eso tenemos **fabricate** una extensión mas profesional para Uds.

---


![image](https://github.com/user-attachments/assets/885ae0a5-0636-4a6c-90aa-aaee7292263b)

Cyberduck es un cliente de transferencia de archivos (FTP/SFTP) y almacenamiento en la nube de código abierto. Es ampliamente utilizado para conectar y gestionar archivos en servidores remotos y servicios en la nube como Amazon S3, Google Drive, OneDrive, Dropbox, y más.

Características principales de Cyberduck:
- ✅ Soporte para múltiples protocolos: Compatible con FTP, SFTP, WebDAV, Amazon S3, OpenStack Swift, Backblaze B2, Microsoft Azure, Google Cloud Storage, Dropbox y OneDrive.
- ✅ Interfaz intuitiva: Diseñado para ser fácil de usar con una navegación sencilla y soporte para arrastrar y soltar.
- ✅ Seguridad avanzada: Soporta encriptación de datos y autenticación mediante claves SSH o credenciales almacenadas en Keychain (macOS) o Windows Credential Manager.
- ✅ Editor de archivos en vivo: Permite editar archivos directamente en el servidor con tu editor de código favorito.
- ✅ Sincronización y automatización: Funciona bien con herramientas como rsync y permite la sincronización de archivos entre servidores y nubes.
- ✅ Gratis y de código abierto: Aunque ofrece una versión de donación, su funcionalidad completa es accesible sin costo.

Casos de uso:
- 🔹 Administrar servidores web mediante FTP/SFTP.
- 🔹 Transferir archivos grandes a servicios en la nube.
- 🔹 Gestionar backups en Amazon S3 o Google Drive.
- 🔹 Conectar múltiples servicios de almacenamiento en un solo lugar.
- 🔹 Editar archivos en servidores de forma remota sin descargarlos.

---
# Herramientas para el modelo fisico, 

LOOM de como configurar VSCODE con la llave de AWS e instalar Ciberduck para subir los archivos
- Generador de BD relacionales basados el mockaroo es https://fabricate.mockaroo.com
- Cyberduck https://cyberduck.io (otras alternativas Filezilla, Termius y otros soportan SFTP)
- VSCODE https://code.visualstudio.com con la extesión SSH
- AWS Academy https://www.awsacademy.com/vforcesite/LMS_Login trabajaremos con **UBUNTU 22.LTS** (no versión 24, hubo eventos que invalidan una opcion)


# AWSAcademy usar UBUNTU 22 (por el momento falta un fix al script en Ubuntu 24.x de AWS)
- 4 Gb RAM
- Descargar el git de Teddy Sun via https://github.com/teddysun/lamp?tab=readme-ov-file#installation
- Comprobacion que funciono:
[https://www.loom.com/share/dba54364e4cd4c6c93226ef04134fc7a?sid=f35a37ff-fec8-4a03-ae42-90cf196db9d9](https://www.loom.com/share/c463ea940e5d40068fa3eafbdab39458?sid=bb48c9b5-fd0e-4085-85bd-3ff6ce4d8d5d)

---

Acceder a nodo con VSCode y subir archivos con CyberDuck:

https://www.loom.com/share/ed3e6ae8380b44288ac912a32e067baa?sid=1a5e7030-0141-4bf1-9edf-9dc4d43d0009


