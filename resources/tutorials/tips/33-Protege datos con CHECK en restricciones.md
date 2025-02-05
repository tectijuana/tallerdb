
# PADRON JIMENEZ NICOL JHAMYLETD


## 📌 1. ¿Qué es una Restricción CHECK?

Una restricción **CHECK** impide que se inserten o actualicen valores que no cumplan con una condición específica. SQL Server verificará automáticamente la condición en cada **INSERT** o **UPDATE**.

---

### 📌 2. Sintaxis de CHECK

### 🔹 Definir CHECK al Crear una Tabla
```sql
CREATE TABLE Productos (
    ProductoID INT PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Precio DECIMAL(10,2) NOT NULL CHECK (Precio > 0)
);

```
### 🔹 Agregar CHECK a una tabla existente
```sql
ALTER TABLE Productos 
ADD CONSTRAINT CHK_Precio CHECK (Precio > 0);

```
### 📌 3. Tipos de Restricciones CHECK
✅ CHECK de Columna se aplica a un solo campo:
```sql
CHECK (Edad >= 18)

```
#### ✅ CHECK de tabla  puede validar múltiples columnas:
```sql
CHECK (FechaInicio <= FechaFin)

```
### 📌 4. Beneficios del Uso de CHECK

#### ✔ Automatización: Evita validaciones manuales en la aplicación.
#### ✔ Seguridad: Protege los datos de valores incorrectos o no deseados.
#### ✔ Optimización: SQL Server puede aprovechar estas restricciones para mejorar el rendimiento​.
```

```
### 📌 5. Consideraciones al Usar CHECK
#### 🚨 No se permiten subconsultas dentro de CHECK (SQL Server).
#### 🚨 Evita funciones dentro de CHECK para no afectar el rendimiento​.
#### 🚨 Si omites WITH CHECK, la restricción solo aplicará a nuevos datos​.
#### 🚨 Si agregas una restricción CHECK después de insertar datos, usa WITH CHECK:
```sql
ALTER TABLE Productos 
WITH CHECK CHECK CONSTRAINT CHK_Precio;

```
## 📌 6. Ejemplo Avanzado: Validación de Formato
### Para garantizar que los códigos de producto sigan un formato específico:
```sql
ALTER TABLE Productos 
ADD CONSTRAINT CHK_CodigoFormato 
CHECK (CodigoProducto LIKE '[A-Z][A-Z][0-9][0-9][0-9]');
```
### 🔹 Esto obliga a que el código comience con dos letras seguidas de tres números​.


