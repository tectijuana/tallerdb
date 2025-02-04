

### 💡 Usa **CASE** para crear columnas condicionales

#### ¿Qué es `CASE`?
`CASE` es una expresión que te permite realizar **evaluaciones condicionales** y devolver diferentes valores según las condiciones. Es como un `IF-ELSE` en SQL y puede ser usado en cualquier parte de una consulta, como en `SELECT`, `WHERE`, o incluso en `ORDER BY`.

---

### Ejemplo práctico:

Supongamos que tienes una tabla `ventas` con las columnas `producto`, `monto_ventas`, y `fecha`. Quieres clasificar las ventas en tres categorías:
- `"Alta"` si el monto es mayor o igual a 1000.
- `"Media"` si está entre 500 y 999.
- `"Baja"` si es menor a 500.

#### Usando `CASE` para clasificar las ventas:
```sql
SELECT producto,
       monto_ventas,
       CASE 
           WHEN monto_ventas >= 1000 THEN 'Alta'
           WHEN monto_ventas BETWEEN 500 AND 999 THEN 'Media'
           ELSE 'Baja'
       END AS categoria
FROM ventas;
```

👉 Esto genera un resultado como este:

| producto | monto_ventas | categoria |
|----------|--------------|-----------|
| Laptop   | 1200         | Alta      |
| Tablet   | 800          | Media     |
| Teclado  | 300          | Baja      |

---

### Más usos de `CASE`:

#### 1. Crear una columna booleana (Sí/No):
Por ejemplo, marcar si una venta ocurrió después de una fecha específica:
```sql
SELECT producto,
       fecha,
       CASE 
           WHEN fecha >= '2025-01-01' THEN 'Sí'
           ELSE 'No'
       END AS venta_reciente
FROM ventas;
```

#### 2. Usarlo en un `WHERE`:
Filtrar productos de categoría "Alta":
```sql
SELECT * 
FROM ventas
WHERE 
    CASE 
        WHEN monto_ventas >= 1000 THEN 'Alta'
        ELSE 'Otra'
    END = 'Alta';
```

#### 3. Ordenar resultados dinámicamente:
Priorizar productos con mayores ventas primero, luego los de categoría "Media":
```sql
SELECT producto, monto_ventas
FROM ventas
ORDER BY 
    CASE 
        WHEN monto_ventas >= 1000 THEN 1
        WHEN monto_ventas BETWEEN 500 AND 999 THEN 2
        ELSE 3
    END;
```

---

### Ventajas de `CASE`:
1. **Flexibilidad**: Útil para transformar datos directamente en la consulta.
2. **Ahorra pasos**: Reduce la necesidad de crear columnas calculadas adicionales.
3. **Claridad**: Hace que las consultas sean fáciles de entender al expresar la lógica directamente.

---

💡 **Tip Extra**: Usa `CASE` dentro de agregados para cálculos condicionales:
```sql
SELECT 
    SUM(CASE WHEN monto_ventas >= 1000 THEN monto_ventas ELSE 0 END) AS total_alta,
    SUM(CASE WHEN monto_ventas < 1000 THEN monto_ventas ELSE 0 END) AS total_otra
FROM ventas;
```

¡Con `CASE`, puedes manejar casi cualquier lógica condicional en tus consultas SQL! 🚀
