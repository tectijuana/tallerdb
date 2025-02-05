# Ulises Hernandez Bojorquez

## 📌 Ordena con ORDER BY y múltiples criterios en SQL

En SQL, la cláusula `ORDER BY` se usa para ordenar los resultados de una consulta. 
Puedes ordenar por una o varias columnas, especificando el orden ascendente (`ASC`) o descendente (`DESC`).

### 🔹 Ejemplo básico:
Ordenar una lista de productos por precio de menor a mayor:

```sql
SELECT nombre, precio 
FROM productos 
ORDER BY precio ASC;
```

### 🔹 Ordenar por múltiples criterios:
Podemos ordenar primero por categoría y luego por precio dentro de cada categoría:

```sql
SELECT nombre, categoria, precio 
FROM productos 
ORDER BY categoria ASC, precio DESC;
```

🔹 **Explicación**:
1. `categoria ASC`: Ordena alfabéticamente las categorías (A-Z).
2. `precio DESC`: Dentro de cada categoría, ordena los productos de mayor a menor precio.

📌 **Tip:** También puedes ordenar por una columna calculada o por su posición en la consulta:

```sql
SELECT nombre, precio, (precio * 0.85) AS precio_descuento
FROM productos 
ORDER BY precio_descuento DESC;
```

### ✅ Beneficios:
✔ Facilita la organización y visualización de datos.  
✔ Optimiza reportes y análisis.  
✔ Se puede combinar con `LIMIT` para paginación eficiente.

¡Ordena tus datos con `ORDER BY` y mejora la presentación de tus consultas SQL! 🚀

Este contenido es claro, conciso y útil para quienes están aprendiendo SQL. ¡Espero que te ayude! 😊
