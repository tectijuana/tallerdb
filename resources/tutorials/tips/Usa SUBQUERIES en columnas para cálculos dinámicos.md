

### 💡 Usa **SUBQUERIES** en columnas para cálculos dinámicos

#### ¿Qué son las subconsultas en columnas?
Las subconsultas en columnas te permiten realizar cálculos dinámicos o recuperar valores específicos directamente dentro de un `SELECT`. Esto es muy útil para agregar datos relacionados sin hacer un `JOIN` completo o para cálculos específicos en cada fila.

---

### Ejemplo práctico:

Supongamos que tienes dos tablas:
1. `empleados`: Contiene `empleado_id`, `nombre` y `departamento_id`.
2. `proyectos`: Contiene `departamento_id` y `cantidad_proyectos`.

Queremos:
1. Mostrar el nombre de cada empleado.
2. Añadir la cantidad de proyectos que tiene su departamento.

#### Solución con una subconsulta en columna:
```sql
SELECT e.nombre,
       e.departamento_id,
       (SELECT COUNT(*)
        FROM proyectos p
        WHERE p.departamento_id = e.departamento_id) AS total_proyectos
FROM empleados e;
```

👉 Esto agrega una columna dinámica con el total de proyectos de cada departamento.

| nombre     | departamento_id | total_proyectos |
|------------|-----------------|-----------------|
| Ana        | 1               | 5               |
| Luis       | 2               | 3               |
| Sofía      | 1               | 5               |

---

### Más ejemplos de subconsultas en columnas:

#### 1. Mostrar el salario más alto en el departamento de cada empleado:
```sql
SELECT nombre,
       departamento_id,
       (SELECT MAX(salario)
        FROM empleados e2
        WHERE e2.departamento_id = e.departamento_id) AS salario_maximo
FROM empleados e;
```

#### 2. Calcular un promedio y comparar cada valor:
Supongamos que tienes una tabla `ventas` y quieres comparar las ventas de cada producto con el promedio general:
```sql
SELECT producto,
       monto_ventas,
       (SELECT AVG(monto_ventas)
        FROM ventas) AS promedio_ventas,
       monto_ventas - (SELECT AVG(monto_ventas) FROM ventas) AS diferencia
FROM ventas;
```

---

### Ventajas de las subconsultas en columnas:
1. **Flexibilidad**: No necesitas hacer múltiples uniones (`JOIN`) si solo necesitas un cálculo puntual.
2. **Reutilización**: Puedes incluir lógicas dinámicas en la misma consulta.
3. **Ahorro de tiempo**: Simplifican consultas donde un `JOIN` puede ser excesivo.

---

### Consideraciones:
- **Performance**: Si usas subconsultas en tablas grandes, pueden ser más lentas. En esos casos, evalúa usar un `JOIN` o una **CTE** para mejorar el rendimiento.
- **Legibilidad**: Aunque son prácticas, pueden hacer la consulta más difícil de leer si son muy complejas.

---

💡 **Tip Extra**: Combina subconsultas con funciones de ventana para aún más poder analítico.

Con este truco, puedes agregar lógica poderosa y personalización dinámica a tus consultas SQL. ¡Aprovecha su flexibilidad! 🚀
