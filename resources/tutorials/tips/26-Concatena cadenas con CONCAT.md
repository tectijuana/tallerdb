*Martinez Guzman Leonardo Rafael 22210318*

### **¿Qué hace la función `CONCAT`?**
La función `CONCAT` en SQL se utiliza para unir (o concatenar) dos o más cadenas de texto en una sola cadena de texto. Esto puede ser útil cuando necesitas combinar valores de diferentes columnas, agregar prefijos o sufijos, o crear nuevos valores a partir de datos existentes.

### **Sintaxis Básica de `CONCAT`:**

```sql
CONCAT(cadena1, cadena2, ..., cadenaN)
```

- **cadena1, cadena2, ..., cadenaN**: Son las cadenas de texto o las columnas de la tabla que deseas concatenar. Puedes concatenar múltiples valores separados por comas.

### **Ejemplo básico de concatenación:**

```sql
SELECT CONCAT('Hola', ' ', 'Mundo') AS Saludo;
```

**Resultado:**
```
Saludo
-------
Hola Mundo
```

### **Concatenando columnas de una tabla:**

Si tienes una tabla con varias columnas, puedes usar `CONCAT` para combinarlas. Por ejemplo, supón que tienes una tabla `empleados` con las columnas `nombre` y `apellido`, y quieres mostrar el nombre completo:

```sql
SELECT CONCAT(nombre, ' ', apellido) AS NombreCompleto
FROM empleados;
```

**Resultado:**
```
NombreCompleto
--------------
Juan Pérez
Ana Gómez
Luis Rodríguez
```

### **Concatenación con valores literales:**

También puedes concatenar texto literal con columnas. Por ejemplo, si quieres mostrar un mensaje con la información de un empleado:

```sql
SELECT CONCAT('El empleado ', nombre, ' ', apellido, ' tiene un puesto de trabajo en la empresa.') AS Mensaje
FROM empleados;
```

**Resultado:**
```
Mensaje
-------------------------------------------------------------
El empleado Juan Pérez tiene un puesto de trabajo en la empresa.
El empleado Ana Gómez tiene un puesto de trabajo en la empresa.
El empleado Luis Rodríguez tiene un puesto de trabajo en la empresa.
```

### **Usando `CONCAT` con valores nulos:**

En SQL, si una de las columnas es `NULL`, la función `CONCAT` tratará el valor `NULL` como una cadena vacía y continuará concatenando los demás valores. Esto es diferente a otras funciones, como `+` en SQL Server, que devolverían `NULL` si alguna columna es `NULL`.

Por ejemplo, si uno de los empleados no tiene apellido, al usar `CONCAT` el resultado no será `NULL`, sino que se concatenará solo el `nombre`:

```sql
SELECT CONCAT(nombre, ' ', apellido) AS NombreCompleto
FROM empleados;
```

**Resultado (si apellido es NULL para algún empleado):**
```
NombreCompleto
--------------
Juan Pérez
Ana NULL
Luis Rodríguez
```

En este caso, si `apellido` es `NULL`, se tratará como una cadena vacía y se devolverá solo el nombre.

### **Concatenación con separadores personalizados:**

Si deseas agregar separadores personalizados (como comas, guiones, etc.), puedes hacerlo fácilmente:

```sql
SELECT CONCAT(nombre, ',', apellido) AS NombreCompleto
FROM empleados;
```

**Resultado:**
```
NombreCompleto
--------------
Juan,Pérez
Ana,Gómez
Luis,Rodríguez
```

### **Consideraciones importantes:**
1. **Compatibilidad con bases de datos**: `CONCAT` es compatible con muchos sistemas de bases de datos como MySQL, PostgreSQL, SQL Server (en versiones más recientes), y otros. Sin embargo, algunas versiones más antiguas de SQL Server usan `+` para concatenar cadenas.
   
2. **Espacios y delimitadores**: Si necesitas controlar cómo se separan los valores, puedes agregar espacios o caracteres adicionales dentro de la función `CONCAT` como se mostró en los ejemplos anteriores.

3. **Longitud de las cadenas**: Dependiendo del sistema de bases de datos, puede haber un límite en la longitud de la cadena resultante. Si necesitas un tamaño más grande, revisa la documentación de tu DBMS para ver cómo manejar cadenas largas.

### **Resumen:**
La función `CONCAT` es una herramienta poderosa y flexible para combinar cadenas de texto en SQL. Puedes usarla para unir columnas, agregar texto estático, y controlar cómo se muestran los resultados, todo dentro de una sola consulta SQL.

