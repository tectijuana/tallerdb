<a href="https://cooltext.com"><img src="https://images.cooltext.com/5727913.png" width="511" height="99" alt="Investigación" /></a>
<br />Image by <a href="https://cooltext.com">Cool Text: Free Graphics Generator</a> - <a href="https://cooltext.com/Edit-Logo?LogoID=4801693220">Edit Image</a>


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
4. Acceso a [http://WhatsMyIp.com](http://[whatsismyip](https://www.whatismyip.com) para obtener IP externa.
5. Acceso a [http://fabricate.mockaroo.com](http://fabricate.mockaroo.com) para generar datos ficticios.

---

## 🪜 Paso a Paso

### 1. Crear instancia EC2 con Windows Server y SQL Server

- **AMI sugerida:** Microsoft Windows Server 2022 Base with SQL Server Web.
- **Tipo de instancia:** t2.medium o superior con 8 Gb RAM
- **Almacenamiento:** mínimo 30 GB (C:).
- **Nombre de clave (key pair):** generar uno nuevo o reutilizar existente para RDP.
- **Grupo de seguridad:**
  - Inbound rules:
    - RDP: TCP 3389 desde *My IP*
    - SQL Server: TCP 1433 desde *My IP*

### 2. Conectar por RDP a la instancia

- **Descargar archivo `.rdp`** desde AWS Console.
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

- Ir a checar la ip en su computadora local.
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

**Contenido mínimo del video (1 hasta 5 minutos):**
1. Acceso a la instancia EC2 vía RDP.
2. Vista de base de datos y datos ficticios en SSMS (dentro del servidor).
3. Configuración del firewall con IP pública.
4. Conexión desde SSMS en su PC local.
5. Ejecución de una consulta.
6. Mostrar para validar IP.

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

# Utilice jetbrains.com DataGrip y/o  SQL Server Manager Studio para acceder a el servidor AWS-MSServer Remoto 1433/tcp para comunicarse con SQL Server 2022 
