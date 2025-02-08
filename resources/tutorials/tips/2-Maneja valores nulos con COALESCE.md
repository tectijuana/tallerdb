### Urrea Gonzalez Fernando Andre
### 💡 Usa **COALESCE** para manejar valores nulos de manera inteligente

#### ¿Qué es `COALESCE`?
`COALESCE` es una función que devuelve el **primer valor no nulo** de una lista. Es súper útil para:
- Reemplazar valores nulos (`NULL`) con un valor predeterminado.
- Evitar errores o cálculos incorrectos causados por valores nulos.

---

### Ejemplo práctico: 

Supongamos que tienes una tabla llamada `clientes` con las columnas `nombre`, `email`, y `telefono`, pero algunos valores en `email` y `telefono` son nulos. Queremos mostrar un contacto preferido (email o teléfono) y si ambos son nulos, mostrar `"Sin contacto"`.

#### Sin usar `COALESCE` (Consulta más larga y menos elegante):
```sql
SELECT nombre,
       CASE 
           WHEN email IS NOT NULL THEN email
           WHEN telefono IS NOT NULL THEN telefono
           ELSE 'Sin contacto'
       END AS contacto_preferido
FROM clientes;
```

#### Con `COALESCE` (Más simple y limpio):
```sql
SELECT nombre,
       COALESCE(email, telefono, 'Sin contacto') AS contacto_preferido
FROM clientes;
```

---

### ¿Cómo funciona?
1. **Email**: Si no es nulo, se usa como `contacto_preferido`.
2. **Teléfono**: Si el email es nulo pero el teléfono no, se toma el teléfono.
3. **Sin contacto**: Si ambos son nulos, se devuelve `'Sin contacto'`.

---

### Ventajas de usar `COALESCE`:
1. **Simplicidad**: Hace que las consultas sean más cortas y fáciles de leer.
2. **Versatilidad**: Funciona con cualquier número de valores en la lista.
3. **Evitar errores**: Garantiza que no te encuentres con valores nulos en los resultados.

---

💡 **Tip Extra**: Usa `COALESCE` también para valores numéricos, como reemplazar valores nulos por `0` en cálculos agregados:

```sql
SELECT producto_id, 
       SUM(COALESCE(cantidad, 0)) AS cantidad_total
FROM ventas
GROUP BY producto_id;
```

Con este truco, nunca más te preocuparás por los valores nulos en tus consultas SQL. ¡Úsalo y optimiza tus resultados! 🚀
