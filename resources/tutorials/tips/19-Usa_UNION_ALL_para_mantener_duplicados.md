## CESAR GONZALEZ SALAZAR - 22211575 

# 💡 **TIP SQL #19:** Usa `UNION ALL` para mantener duplicados.  
Combina los resultados de múltiples consultas **sin eliminar duplicados**,  
a diferencia de `UNION`, que los elimina automáticamente.  

> 🔹 **Ventaja**: `UNION ALL` es más rápido porque no necesita filtrar duplicados.  
> 🔹 **Úsalo**  cuando quieras conservar todos los registros sin modificaciones.

# 🔹 ¿Qué es `UNION ALL` en SQL?
`UNION ALL` es un operador en SQL que se usa para combinar los resultados de dos o más consultas **sin eliminar duplicados**.  
Es diferente de `UNION`, que sí elimina los valores repetidos.

---

## 📌 Diferencia entre `UNION` y `UNION ALL`
| Operador  | Duplicados | Rendimiento |
|-----------|-----------|-------------|
| `UNION`     | Elimina duplicados | Más lento (porque debe ordenar y filtrar los datos) |
| `UNION ALL` | Mantiene duplicados | Más rápido (no necesita ordenar ni eliminar datos) |

---

## 📖 Ejemplo práctico de `UNION ALL`
Supongamos que tenemos dos tablas:  
- **Ventas_2023**
- **Ventas_2024**

Cada tabla tiene una columna `Producto`. Queremos unir ambas listas sin eliminar duplicados.

```sql
SELECT Producto FROM Ventas_2023
UNION ALL
SELECT Producto FROM Ventas_2024;

```
## 🔹 Salida esperada (los duplicados se eliminan):

```sql
Producto
--------
Laptop
Celular
Tablet
```

# 🚀 ¿Cuándo usar UNION ALL?
✅ Cuando necesitas conservar todos los valores, incluso los repetidos.
✅ Cuando el rendimiento es importante, ya que UNION ALL es más rápido que UNION.

🔴 Evita UNION ALL si necesitas una lista de valores únicos

# 🤓 Resumen
UNION elimina duplicados.
UNION ALL mantiene los duplicados y es más rápido.
Úsalo cuando la duplicidad de datos sea relevante en tu análisis.
