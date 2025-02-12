*Autor: 28Samuel28 Esteves Peña Samuel (Num.Control: 22210762)*

# Funcion `CUBE`
La función `CUBE` en SQL Server es un operador que extiende la funcionalidad de GROUP BY permitiendo generar combinaciones de agregaciones para todas las combinaciones posibles de las columnas agrupadas.

Características del operador CUBE:
-Genera todas las combinaciones posibles de agregaciones para los valores seleccionados.
-Incluye subtotales y totales generales en los resultados.
-No permite el uso de la palabra clave DISTINCT en funciones de agregación.

Permite obtener la mayores posibles agrupaciones y totales en una consulta con `GROUP BY`. Similar a la funcion `ROLLUP`, uan de las diferencias entre esta funcion y la segunda es que `ROLLUP` genera subtotales `CUBE` calcula todas las combinaciones posibles de los valores de agrupacion 

--Dentro del primer ejemplo se generara resultados parciales y el resultado general, con las combinaciones de la columna1 y la columna2.

SELECT columna1, columna2, SUM(Columna3) AS Suma
FROM Tabla
GROUP BY CUBE(columna1, columna2);

--Dentro de este ejemplo de crea la tabla de inventario la cual guardara un Ite(objeto), Color y la cantidad que hay, 
CREATE TABLE Inventario (
    Item VARCHAR(50),
    Color VARCHAR(20),
    Cantidad INT
);

--Insertando los valores de la tabla de Inventario para despues para saber si fueron correctamente insertados se Usa SELECT * FROM Inventario;

INSERT INTO Inventario (Item, Color, Cantidad)
VALUES 
    ('Mesa', 'Azul', 124),
    ('Mesa', 'Rojo', 223),
    ('Silla', 'Azul', 101),
    ('Silla', 'Rojo', 210);

SELECT * FROM Inventario;

--Para al final EJECUTAR una consulta de algunos de los datos de la table y tanto de los colores que habia y la cantidad de CADA COLOR como la suma de los 2 colores que se insertaron en la tabla 
SELECT 
    CASE WHEN GROUPING(Item) = 1 THEN 'TOTAL' ELSE Item END AS Item,
    CASE WHEN GROUPING(Color) = 1 THEN 'TOTAL' ELSE Color END AS Color,
    SUM(Cantidad) AS CantidadTotal
FROM Inventario
GROUP BY CUBE(Item, Color);

