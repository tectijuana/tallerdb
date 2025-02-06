# 📊 Función `CUBE` en SQL Server

**Autor:** 28Samuel28 Esteves Peña Samuel  
**Número de Control:** 22210762  

---

## 📌 Introducción

La función `CUBE` en SQL Server es un operador que extiende la funcionalidad de `GROUP BY`, permitiendo generar combinaciones de agregaciones para **todas las combinaciones posibles** de las columnas agrupadas.

---

## 🚀 Características del Operador `CUBE`

- 📦 **Generación de combinaciones:** Produce todas las combinaciones posibles de agregaciones para los valores seleccionados.  
- ➕ **Subtotales y totales generales:** Incluye subtotales y totales generales en los resultados.  
- 🚫 **Restricción:** No permite el uso de la palabra clave `DISTINCT` en funciones de agregación.

> **Diferencia clave:**  
> A diferencia de `ROLLUP`, que solo genera subtotales jerárquicos, `CUBE` calcula todas las combinaciones posibles de los valores de agrupación.

---

## 🗂️ Ejemplo 1: Uso Básico de `CUBE`

```sql
SELECT columna1, columna2, SUM(Columna3) AS Suma
FROM Tabla
GROUP BY CUBE(columna1, columna2);


CREATE TABLE Inventario (
    Item VARCHAR(50),
    Color VARCHAR(20),
    Cantidad INT
);
INSERT INTO Inventario (Item, Color, Cantidad)
VALUES 
    ('Mesa', 'Azul', 124),
    ('Mesa', 'Rojo', 223),
    ('Silla', 'Azul', 101),
    ('Silla', 'Rojo', 210);

SELECT * FROM Inventario;
SELECT 
    CASE WHEN GROUPING(Item) = 1 THEN 'TOTAL' ELSE Item END AS Item,
    CASE WHEN GROUPING(Color) = 1 THEN 'TOTAL' ELSE Color END AS Color,
    SUM(Cantidad) AS CantidadTotal
FROM Inventario
GROUP BY CUBE(Item, Color);


