
### 💡 Usa **GROUPING SETS** para crear resúmenes avanzados

#### ¿Qué son los `GROUPING SETS`?
`GROUPING SETS` es una extensión de `GROUP BY` que te permite calcular múltiples niveles de agregación en una sola consulta. Esto es útil para generar informes jerárquicos o multidimensionales sin necesidad de escribir varias consultas.

---

### Ejemplo práctico:

Supongamos que tienes una tabla `ventas` con las columnas `categoria`, `producto`, y `monto_ventas`. Queremos:
1. Obtener el total de ventas por categoría.
2. Obtener el total de ventas por producto.
3. Obtener el total global (todas las categorías y productos combinados).

#### Sin `GROUPING SETS` (Consulta repetitiva y más código):
```sql
-- Total por categoría
SELECT categoria, NULL AS producto, SUM(monto_ventas) AS total_ventas
FROM ventas
GROUP BY categoria

UNION ALL

-- Total por producto
SELECT NULL AS categoria, producto, SUM(monto_ventas) AS total_ventas
FROM ventas
GROUP BY producto

UNION ALL

-- Total global
SELECT NULL AS categoria, NULL AS producto, SUM(monto_ventas) AS total_ventas
FROM ventas;
```

#### Con `GROUPING SETS` (Sencillo y directo):
```sql
SELECT categoria, 
       producto, 
       SUM(monto_ventas) AS total_ventas
FROM ventas
GROUP BY GROUPING SETS (
    (categoria),        -- Total por categoría
    (producto),         -- Total por producto
    ()                  -- Total global
);
```

👉 Esto genera un resultado como este:

| categoria   | producto   | total_ventas |
|-------------|------------|--------------|
| Electrónica | NULL       | 5000         |
| Ropa        | NULL       | 3000         |
| NULL        | Televisor  | 2000         |
| NULL        | Camisa     | 1500         |
| NULL        | NULL       | 8000         |

---

### Más ejemplos de uso:

#### 1. Generar totales acumulados jerárquicos:
```sql
SELECT categoria, 
       producto, 
       SUM(monto_ventas) AS total_ventas
FROM ventas
GROUP BY GROUPING SETS (
    (categoria, producto), -- Total por categoría y producto
    (categoria),           -- Total por categoría
    ()                     -- Total global
);
```

#### 2. Filtrar resultados con `ROLLUP` o `CUBE`:
- **ROLLUP**: Totaliza jerárquicamente (de más específico a menos específico).
- **CUBE**: Totaliza todas las combinaciones posibles.
```sql
SELECT categoria, 
       producto, 
       SUM(monto_ventas) AS total_ventas
FROM ventas
GROUP BY ROLLUP (categoria, producto);
```

---

### Ventajas de `GROUPING SETS`:
1. **Flexibilidad**: Permite obtener múltiples niveles de detalle en un solo paso.
2. **Ahorro de tiempo**: Menos consultas independientes, más resultados en una sola consulta.
3. **Compatibilidad**: Funciona con otros comandos como `HAVING` para filtrar datos.

---

💡 **Tip Extra**: Usa la función `GROUPING()` para identificar filas que representan totales:
```sql
SELECT categoria, 
       producto, 
       SUM(monto_ventas) AS total_ventas,
       GROUPING(categoria) AS es_total_categoria,
       GROUPING(producto) AS es_total_producto
FROM ventas
GROUP BY GROUPING SETS ((categoria, producto), (categoria), ());
```

Esto te ayuda a distinguir totales globales de subtotales.

---

¡Con `GROUPING SETS`, `ROLLUP` y `CUBE`, puedes construir reportes avanzados y personalizables de manera eficiente! 🚀
