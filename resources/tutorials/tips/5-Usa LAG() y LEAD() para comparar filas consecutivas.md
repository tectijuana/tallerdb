

### 💡 Usa **LAG() y LEAD()** para comparar filas consecutivas

#### ¿Qué son `LAG()` y `LEAD()`?
Son **funciones de ventana** que te permiten acceder a los valores de filas anteriores (`LAG`) o posteriores (`LEAD`) sin necesidad de realizar un `JOIN` o consultas complejas. Estas funciones son ideales para realizar cálculos basados en datos consecutivos.

---

### Ejemplo práctico:

Supongamos que tienes una tabla `ventas` con estas columnas: `fecha`, `producto`, y `monto_ventas`. Quieres:
1. Comparar las ventas de cada día con las del día anterior.
2. Calcular la diferencia entre las ventas actuales y las pasadas.

#### Con `LAG()` (Comparación con el día anterior):
```sql
SELECT fecha,
       producto,
       monto_ventas,
       LAG(monto_ventas) OVER (PARTITION BY producto ORDER BY fecha) AS ventas_anterior,
       monto_ventas - LAG(monto_ventas) OVER (PARTITION BY producto ORDER BY fecha) AS diferencia
FROM ventas;
```

👉 Esto genera un resultado como este:

| fecha       | producto | monto_ventas | ventas_anterior | diferencia |
|-------------|----------|--------------|-----------------|------------|
| 2025-01-01  | A        | 100          | NULL            | NULL       |
| 2025-01-02  | A        | 150          | 100             | 50         |
| 2025-01-03  | A        | 200          | 150             | 50         |

---

#### Con `LEAD()` (Comparación con el día siguiente):
Si deseas comparar con las ventas del día siguiente:
```sql
SELECT fecha,
       producto,
       monto_ventas,
       LEAD(monto_ventas) OVER (PARTITION BY producto ORDER BY fecha) AS ventas_siguiente,
       LEAD(monto_ventas) OVER (PARTITION BY producto ORDER BY fecha) - monto_ventas AS diferencia_siguiente
FROM ventas;
```

👉 Esto genera un resultado como este:

| fecha       | producto | monto_ventas | ventas_siguiente | diferencia_siguiente |
|-------------|----------|--------------|------------------|-----------------------|
| 2025-01-01  | A        | 100          | 150              | 50                    |
| 2025-01-02  | A        | 150          | 200              | 50                    |
| 2025-01-03  | A        | 200          | NULL             | NULL                  |

---

### ¿Cuándo usar estas funciones?
1. **Análisis de tendencias**: Comparar datos consecutivos como ventas diarias, precios, métricas de rendimiento, etc.
2. **Identificar cambios**: Detectar picos, caídas o fluctuaciones entre filas consecutivas.
3. **Análisis de series temporales**: Trabajar con datos organizados por tiempo.

---

### 💡 **Tip Extra**:
Puedes usar `COALESCE()` para manejar valores nulos en los resultados:
```sql
COALESCE(LAG(monto_ventas) OVER (...), 0)
```

---

¡Con `LAG()` y `LEAD()` puedes realizar análisis más profundos y descubrir tendencias ocultas en tus datos! 🚀
