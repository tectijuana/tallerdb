*Martinez Guzman Leonardo Rafael 22210318*

# Concatenación de Cadenas en SQL con `CONCAT`

La función `CONCAT` en SQL se usa para unir dos o más cadenas de texto en un solo resultado. Es útil cuando deseas combinar datos de diferentes columnas o agregar texto adicional en los resultados de una consulta.

## Sintaxis Básica

La sintaxis básica de la función `CONCAT` es la siguiente:

```sql
CONCAT(cadena1, cadena2, ..., cadenaN)
```

- **cadena1, cadena2, ..., cadenaN**: Las cadenas que deseas concatenar. Pueden ser columnas de una tabla o valores literales (como cadenas de texto entre comillas).

## Ejemplos

### 1. Concatenación de dos columnas

Supongamos que tienes una tabla llamada `empleados`, con las columnas `nombre` y `apellido`, y deseas obtener el nombre completo de cada empleado. Puedes usar `CONCAT` de la siguiente manera:

```sql
SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo
FROM empleados;
```

**Explicación:**
- `nombre` y `apellido` son las columnas que contienen los nombres y apellidos de los empleados.
- `' '` (un espacio) se incluye entre las dos columnas para separar el nombre y el apellido.
- El resultado se mostrará en una columna llamada `nombre_completo`.

### 2. Concatenación de columnas con texto adicional

Si deseas agregar texto adicional a los resultados concatenados, puedes hacerlo fácilmente. Por ejemplo, si quieres concatenar el nombre y apellido del empleado y agregar un texto como "Empleado: ", lo harías así:

```sql
SELECT CONCAT('Empleado: ', nombre, ' ', apellido) AS nombre_completo
FROM empleados;
```

**Explicación:**
- `'Empleado: '` es una cadena literal que se agrega antes del nombre y apellido.
- El resultado de la concatenación será algo como: "Empleado: Juan Pérez".

### 3. Concatenación de más de dos columnas

Puedes concatenar tantas columnas como desees. Si, además del nombre y apellido, tienes una columna `departamento`, puedes concatenar las tres de la siguiente manera:

```sql
SELECT CONCAT(nombre, ' ', apellido, ' - ', departamento) AS detalles_empleado
FROM empleados;
```

**Explicación:**
- `nombre`, `apellido` y `departamento` son las columnas que estamos concatenando.
- Se usa `' - '` para separar el apellido del departamento.
- El resultado será algo como: "Juan Pérez - Finanzas".

## 4. Uso de `CONCAT` con valores literales

A veces, puedes querer agregar texto fijo (no solo de columnas) a los resultados. Por ejemplo, si deseas incluir un mensaje personalizado en el resultado, puedes hacerlo de esta manera:

```sql
SELECT CONCAT('El empleado ', nombre, ' ', apellido, ' trabaja en el departamento de ', departamento) AS descripcion
FROM empleados;
```

**Explicación:**
- El mensaje completo en la columna `descripcion` puede ser algo como: "El empleado Juan Pérez trabaja en el departamento de Finanzas".

## 5. `CONCAT` en SQL Server

En SQL Server, la función `CONCAT` está disponible, pero también puedes usar el operador `+` para concatenar cadenas. Aquí te muestro cómo hacerlo:

```sql
-- Usando CONCAT en SQL Server
SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo
FROM empleados;

-- Usando el operador '+' en SQL Server
SELECT nombre + ' ' + apellido AS nombre_completo
FROM empleados;
```

Ambos enfoques harán lo mismo, pero si una de las columnas contiene un valor `NULL`, el operador `+` devolverá `NULL` en lugar de la cadena concatenada. `CONCAT` manejará `NULL` de manera diferente y lo ignorará.

## 6. Consideraciones sobre `NULL`

Cuando trabajas con datos que pueden contener valores `NULL`, es importante saber cómo manejar estos valores:

- **Con `CONCAT`**: Si una de las cadenas es `NULL`, `CONCAT` la ignorará y solo concatenará las cadenas no nulas.
  
  ```sql
  SELECT CONCAT('Empleado: ', nombre, ' ', apellido, ' - ', departamento) AS nombre_completo
  FROM empleados;
  ```
  En este caso, si `departamento` es `NULL`, el resultado aún se mostrará correctamente, como "Empleado: Juan Pérez - NULL".

- **Con el operador `+` en SQL Server**: Si una de las columnas es `NULL`, el resultado de la concatenación será `NULL`.

  ```sql
  SELECT nombre + ' ' + apellido + ' - ' + departamento AS nombre_completo
  FROM empleados;
  ```

  En este caso, si cualquier columna es `NULL`, el resultado completo será `NULL`.

---

## Resumen

- **`CONCAT`**: Utilízalo para concatenar múltiples cadenas o columnas, ignorando los valores `NULL`.
- **SQL Server**: Puedes usar `CONCAT` o el operador `+` para concatenar. Asegúrate de manejar correctamente los valores `NULL`.
- **Agregar texto adicional**: Puedes añadir texto literal entre las columnas que estás concatenando.

Este es un uso básico y algunos ejemplos más avanzados de cómo usar `CONCAT` en SQL para crear consultas más efectivas y limpias.

