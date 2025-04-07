
# 🧑‍🏫 Lección: Implementación de Windows Server en AWS EC2 con SQL Server y Consulta Remota desde Cliente Local

### 🎯 Objetivos de Aprendizaje (Learning Objectives)
- Implementar una instancia EC2 con Windows Server y SQL Server.
- Importar datos ficticios desde Mockaroo.
- Configurar SQL Server para acceso remoto.
- Configurar reglas de seguridad (firewall) en AWS para permitir acceso desde IP externa.
- Realizar una conexión remota desde SQL Server Management Studio (SSMS) en su PC local.
- Documentar el proceso en una grabación con LOOM.

---

## 🧰 Requisitos Previos

1. Cuenta activa de AWS Academy.
2. SQL Server Management Studio (SSMS) instalado en equipo local.
3. Cuenta gratuita de LOOM.
4. Acceso a [http://checkmyip.com](http://checkmyip.com) para obtener IP externa.
5. Acceso a [http://fabricate.mockaroo.com](http://fabricate.mockaroo.com) para generar datos ficticios.

---

## 🪜 Paso a Paso

### 1. Crear instancia EC2 con Windows Server y SQL Server

- **AMI sugerida:** Microsoft Windows Server 2019 Base with SQL Server Express.
- **Tipo de instancia:** t2.medium o superior.
- **Almacenamiento:** mínimo 30 GB (C:).
- **Nombre de clave (key pair):** generar uno nuevo o reutilizar existente para RDP.
- **Grupo de seguridad:**
  - Inbound rules:
    - RDP: TCP 3389 desde *My IP*
    - SQL Server: TCP 1433 desde *My IP*

### 2. Conectar por RDP a la instancia

- Descargar archivo `.rdp` desde AWS Console.
- Usar contraseña de administrador obtenida mediante clave PEM.

### 3. Habilitar SQL Server para conexiones remotas

Dentro del servidor (vía RDP):

- Abrir **SQL Server Configuration Manager**:
  - Habilitar **TCP/IP** en "SQL Server Network Configuration".
  - En propiedades de TCP/IP, verificar puerto 1433.
- Reiniciar el servicio de SQL Server.

### 4. Crear base de datos y cargar datos ficticios

- Desde [http://fabricate.mockaroo.com](http://fabricate.mockaroo.com):
  - Diseñar un esquema (ej. `clientes`, `ventas`, etc.).
  - Descargar en formato `.csv`.
- En SSMS (dentro del servidor):
  - Crear base de datos: `CREATE DATABASE DemoDB;`
  - Crear tablas e importar CSV:
    ```sql
    BULK INSERT DemoDB.dbo.Clientes
    FROM 'C:\Users\Administrator\Downloads\clientes.csv'
    WITH (
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n',
        FIRSTROW = 2
    );
    ```

### 5. Configurar IP del cliente en el Security Group

- Ir a [http://checkmyip.com](http://checkmyip.com) en su computadora local.
- Copiar IP pública.
- En el **Security Group** de EC2:
  - Editar Inbound Rules → Agregar TCP 1433 desde la IP obtenida (formato: `X.X.X.X/32`).

### 6. Conectarse desde SSMS en la computadora local

- Abrir SSMS.
- En "Server name": `X.X.X.X,1433` (IP pública de la EC2).
- Autenticación: SQL Server Authentication (crear login en SQL Server si no se usa el de Windows).
- Verificar acceso y ejecutar consulta de prueba:
  ```sql
  SELECT TOP 10 * FROM DemoDB.dbo.Clientes;
  ```

---

## 🎥 Entrega de Video en LOOM

**Contenido mínimo del video (3-5 minutos):**
1. Acceso a la instancia EC2 vía RDP.
2. Vista de base de datos y datos ficticios en SSMS (dentro del servidor).
3. Configuración del firewall con IP pública.
4. Conexión desde SSMS en su PC local.
5. Ejecución de una consulta.
6. Mostrar página [checkmyip.com] para validar IP.

---

## ✅ Criterios de Evaluación

| Criterio                        | Peso |
|-------------------------------|------|
| Instancia funcional            | 20%  |
| Importación de datos correcta | 20%  |
| Configuración de red segura   | 20%  |
| Conexión remota exitosa       | 20%  |
| Entrega clara en LOOM         | 20%  |

---

Se presenta un **template de script SQL** para usar con datos generados en [https://fabricate.mockaroo.com](https://fabricate.mockaroo.com), orientado a una tabla ficticia de `Clientes`. Este script está preparado para su uso en SQL Server Express sobre Windows Server en AWS EC2.

---

## 📄 1. Definición de Tabla `Clientes`

```sql
USE DemoDB;
GO

CREATE TABLE dbo.Clientes (
    ClienteID INT PRIMARY KEY,
    Nombre NVARCHAR(100),
    Apellido NVARCHAR(100),
    Email NVARCHAR(255),
    Telefono NVARCHAR(20),
    Ciudad NVARCHAR(100),
    Pais NVARCHAR(100),
    FechaRegistro DATE
);
```

> ⚠️ Asegúrate que las columnas coincidan con el formato de columnas descargado desde Mockaroo.

---

## 📥 2. Importación desde archivo CSV

Ubica el archivo descargado (`clientes.csv`) en una ruta accesible, por ejemplo:  
`C:\Datos\clientes.csv`

> Puedes moverlo con RDP al escritorio y luego copiarlo a `C:\Datos\`.

### Recomendación: Crear la carpeta si no existe

```cmd
mkdir C:\Datos
```

---

### Comando BULK INSERT

```sql
BULK INSERT dbo.Clientes
FROM 'C:\Datos\clientes.csv'
WITH (
    FIRSTROW = 2,
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    TABLOCK,
    ERRORFILE = 'C:\Datos\clientes_err.log'
);
```

---

## 🧪 3. Consulta de Verificación

```sql
SELECT TOP 10 * FROM dbo.Clientes;
```

---

## ✅ Notas y Recomendaciones

- Asegúrate que el CSV no tenga encabezados con caracteres especiales.
- Usa *Notepad++* o *Excel* para validar formato del CSV.
- La codificación del archivo debe ser UTF-8 (sin BOM) si contiene caracteres especiales.

---

