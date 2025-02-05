### Urrea Gonzalez Fernando Andre
### 💡 Usa las CTEs (Expresiones de Tabla Común) para simplificar consultas complejas

#### ¿Qué es una CTE?
Una CTE es un conjunto de resultados temporal que puedes referenciar en una consulta `SELECT`, `INSERT`, `UPDATE` o `DELETE`. Te ayuda a hacer que tus consultas sean más **legibles** y **modulares**.

---

### Ejemplo: Desglosar una consulta compleja

Imagina que queremos:
1. Calcular las ventas totales de cada vendedor.
2. Encontrar los vendedores cuyas ventas totales superan las ventas promedio.

#### Sin una CTE (Consulta confusa y difícil de leer):
```sql
SELECT vendedor_id, ventas_totales
FROM (
    SELECT vendedor_id, SUM(monto_ventas) AS ventas_totales
    FROM ventas
    GROUP BY vendedor_id
) subconsulta
WHERE ventas_totales > (
    SELECT AVG(ventas_totales)
    FROM (
        SELECT SUM(monto_ventas) AS ventas_totales
        FROM ventas
        GROUP BY vendedor_id
    ) subconsulta2
);
```

#### Con una CTE (Consulta más clara y ordenada):
```sql
WITH VentasTotales AS (
    SELECT vendedor_id, SUM(monto_ventas) AS ventas_totales
    FROM ventas
    GROUP BY vendedor_id
),
PromedioVentas AS (
    SELECT AVG(ventas_totales) AS promedio_ventas
    FROM VentasTotales
)
SELECT vendedor_id, ventas_totales
FROM VentasTotales
WHERE ventas_totales > (SELECT promedio_ventas FROM PromedioVentas);
```

---

### Ventajas de usar CTEs:
1. **Legibilidad**: Divide la consulta en partes lógicas y claras.
2. **Reutilización**: Puedes usar una CTE varias veces en una misma consulta.
3. **Depuración**: Es más fácil probar cada parte de la consulta.

💡 **Tip Extra**: También puedes encadenar varias CTEs o usar **CTEs recursivas** para construir jerarquías (como estructuras organizacionales).
