# Rosa Barajas
## *Tip*: Usa DISTINCT para eliminar duplicados en tus consultas SQL

Cuando trabajas con bases de datos relacionales, es común que las consultas devuelvan registros duplicados, especialmente cuando se realizan uniones de tablas o cuando no se han aplicado restricciones adecuadas. Para asegurarte de obtener solo valores únicos en tu consulta, puedes usar la cláusula `DISTINCT` en SQL.

Por ejemplo, si tienes una tabla de clientes y quieres obtener una lista de ciudades sin repetir, puedes usar la siguiente consulta:

```sql
SELECT DISTINCT ciudad FROM clientes;
```
👉 Esto devolverá solo los nombres de ciudades únicos en lugar de repetirlos por cada cliente.

### 1. Elimina filas duplicadas en los resultados
Cuando realizas una consulta con `SELECT`, es posible que algunas filas tengan valores repetidos en todas sus columnas. Para evitar esto, puedes utilizar `DISTINCT`.
```sql
SELECT DISTINCT VendorCity, VendorState 
FROM Vendors;
```
👉 En este ejemplo, cada combinación única de `VendorCity` y `VendorState` aparecerá solo una vez en el resultado​.

### 2. Funciona en el nivel de fila, no de columna
`DISTINCT` aplica a toda la fila en el conjunto de resultados, no solo a una columna individual. Si seleccionas múltiples columnas, eliminará solo las filas completamente duplicadas, no valores repetidos de una sola columna​.

```sql
SELECT DISTINCT name, price FROM CAR;
```
👉 Esto asegurará que la combinación de name y price sea única en los resultados.

### 3. No se debe confundir con GROUP BY
Aunque `DISTINCT` y `GROUP BY` pueden dar resultados similares, `GROUP BY` se usa principalmente con funciones agregadas (por ejemplo, `COUNT`, `SUM`, `AVG`). Si solo deseas obtener valores únicos, `DISTINCT` es la mejor opción​.

### 4. Ordenación implícita en algunos motores de bases de datos
En SQL Server, cuando se usa `DISTINCT`, los resultados suelen ordenarse automáticamente según la primera columna seleccionada, aunque no es una regla garantizada en todos los sistemas de bases de datos​.

### 5. Uso con funciones agregadas
Si necesitas contar valores únicos dentro de una columna, puedes combinar `DISTINCT` con `COUNT`:
```sql
SELECT COUNT(DISTINCT VendoriD) AS NumberOfVendors
FROM Invoices
WHERE InvoiceDate > '2019-07-01';
```
👉Esto cuenta la cantidad de `VendoriD` únicos en la tabla de `Invoices` después de la fecha especificada​.

---
## Cuándo usar DISTINCT:
- ✅ Cuando necesitas una lista de valores únicos en una columna.
- ✅ Para evitar datos redundantes en reportes y análisis.
- ✅ Cuando no puedes modificar la estructura de la base de datos pero necesitas limpiar los resultados de una consulta.

## Consideraciones:
- ⚠️ `DISTINCT` puede afectar el rendimiento en tablas grandes, ya que la base de datos debe procesar y filtrar los valores únicos.
- ⚠️ Si necesitas eliminar duplicados de forma permanente, considera mejorar la normalización de tu base de datos o usar técnicas como GROUP BY según el contexto.

💡 ¡Aprovecha `DISTINCT` para mejorar la precisión de tus consultas y hacer más eficiente la presentación de datos! 🚀

---
## Conclusión ⭐
El uso de `DISTINCT` es una forma sencilla y eficiente de evitar duplicados en los resultados de una consulta. Sin embargo, es importante recordar que trabaja a nivel de fila completa y que, en algunos casos, `GROUP BY` puede ser una mejor opción cuando se necesita realizar cálculos sobre los datos.
