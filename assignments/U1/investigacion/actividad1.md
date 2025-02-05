
# Función `SUM`

*Autor: VTorres2024(No. de Control 21212058)*

La función `SUM` en SQL se utiliza para calcular la suma total de los valores en una columna específica de una tabla, siempre que esta columna contenga datos numéricos.
Es una función de agregación comúnmente usada en consultas para obtener totales de ventas, ingresos, cantidades u otros valores numéricos acumulativos.

Se puede emplear tanto en consultas simples, donde devuelve la suma total de una columna, como en consultas con `GROUP BY`, permitiendo calcular sumas agrupadas por una o más categorías.
Por ejemplo, si se tiene una tabla de ventas con una columna monto, `SUM(monto)` calculará el total de todas las ventas, mientras que `SUM(monto) GROUP BY categoria` mostrará la suma de ventas separada por categoría de productos.

```
-- Crear la tabla ventas
CREATE TABLE ventas (
    id INT PRIMARY KEY,        -- Identificador único de la venta
    categoria VARCHAR(50),     -- Categoría del producto
    monto DECIMAL(10,2),       -- Monto de la venta
    fecha DATE                 -- Fecha de la venta
);

-- Insertar datos de ejemplo
INSERT INTO ventas (id, categoria, monto, fecha) VALUES
    (1, 'Electrónica', 200.50, '2024-02-01'),
    (2, 'Electrónica', 150.75, '2024-02-02'),
    (3, 'Ropa', 80.20, '2024-02-01'),
    (4, 'Ropa', 120.90, '2024-02-03'),
    (5, 'Hogar', 300.00, '2024-02-02');

-- Calcular el total de ventas
SELECT SUM(monto) AS total_ventas FROM ventas;

-- Calcular el total de ventas por categoría
SELECT categoria, SUM(monto) AS total_ventas
FROM ventas
GROUP BY categoria;
```

