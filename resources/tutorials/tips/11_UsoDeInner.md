# Alumno: Corza Morales Ian Kurt - 22211544
# Uso de INNER JOIN para Combinaciones Precisamente en SQL

El **INNER JOIN** es una de las formas más comunes de combinar datos de dos o más tablas en SQL. Se usa para obtener solo aquellas filas donde hay coincidencias en ambas tablas, basándose en una condición especificada.

## 🔹 ¿Cómo Funciona INNER JOIN?

El `INNER JOIN` compara los valores de una columna en la primera tabla con los valores de una columna en la segunda tabla. Si los valores coinciden, se devuelve la fila; de lo contrario, la fila no se incluye en el resultado.

## 🔹 Sintaxis Básica

```sql
SELECT columna1, columna2, ...
FROM tabla1
INNER JOIN tabla2
ON tabla1.columna_comun = tabla2.columna_comun;
 📌 Nota: INNER JOIN y JOIN son equivalentes; el prefijo INNER es opcional.

##🔹 Ejemplo Práctico
Supongamos que tenemos dos tablas:

Clientes (ClienteID, Nombre)
Pedidos (PedidoID, ClienteID, FechaPedido)
Para obtener la lista de clientes con sus pedidos, usamos:

sql
Copiar
Editar
SELECT Clientes.Nombre, Pedidos.PedidoID, Pedidos.FechaPedido
FROM Clientes
INNER JOIN Pedidos
ON Clientes.ClienteID = Pedidos.ClienteID;
Este JOIN devuelve solo los clientes que tienen pedidos registrados. Los clientes sin pedidos no aparecerán en el resultado.

##🔹 Beneficios de INNER JOIN
✔ Filtra y muestra solo las coincidencias entre las tablas.
✔ Optimiza el rendimiento al trabajar con grandes volúmenes de datos.
✔ Evita la duplicación de información redundante.

##🔹 INNER JOIN con Múltiples Tablas
Puedes unir más de dos tablas de la siguiente manera:

sql
Copiar
Editar
SELECT Clientes.Nombre, Pedidos.PedidoID, Productos.Nombre AS Producto
FROM Clientes
INNER JOIN Pedidos ON Clientes.ClienteID = Pedidos.ClienteID
INNER JOIN Productos ON Pedidos.ProductoID = Productos.ProductoID;
Esto devuelve una lista de clientes con sus pedidos y los productos que pidieron.

##🔹 INNER JOIN con Alias
Usar alias (con AS) hace que el código sea más legible:

sql
Copiar
Editar
SELECT c.Nombre, p.PedidoID
FROM Clientes AS c
JOIN Pedidos AS p
ON c.ClienteID = p.ClienteID;
💡 Los alias son útiles en consultas complejas con nombres de tablas largos.

#🔹 Resumen
INNER JOIN devuelve solo los registros coincidentes.
Se usa en combinación con ON para definir la condición de unión.
Se pueden unir múltiples tablas en una consulta.
Los alias facilitan la lectura y el mantenimiento del código.
