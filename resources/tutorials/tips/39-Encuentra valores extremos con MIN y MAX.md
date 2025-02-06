# ROJAS OSUNA JORDHY JAIR

## 📌 Encuentra valores extremos con MIN y MAX

La paginación en SQL se puede lograr utilizando la cláusula `OFFSET` junto con `FETCH` o `LIMIT`, dependiendo del motor de base de datos que estés utilizando.

### **Paginación en SQL Server**
estas funciones se pueden aplicar a columnas de tipo numérico, de texto o fecha.
- MIN(columna): Devuelve el valor mínimo dentro de la columna especificada.
- MAX(columna): Devuelve el valor máximo dentro de la columna especificada.

## **Optimización de consultas con funciones agregadas `MIN()` y `MAX()`**
✅ Eficiencia:
- `MIN()` y `MAX()` permiten obtener los valores extremos sin necesidad de recorrer manualmente la tabla.
- Se ejecutan en tiempo constante O(n) en bases de datos indexadas, lo que las hace eficientes en grandes volúmenes de datos.

✅ Menos procesamiento en la aplicación:
- Evita traer todos los datos a la capa de aplicación y realizar cálculos manuales, reduciendo la carga de procesamiento en el servidor.

## **Uso de `GROUP BY` para análisis por categorías**

✅ Mejor interpretación de datos:
- Permite obtener el salario mínimo y máximo por departamento, facilitando la toma de decisiones.
- Se puede aplicar a cualquier categoría (por ejemplo, ventas, regiones, etc.), lo que lo hace escalable.
  
✅ Reducción del procesamiento manual:
- En lugar de hacer múltiples consultas por cada departamento, GROUP BY automatiza este análisis en una sola ejecución.


## **Ejemplos**
Ejemplo con valores numéricos:
```sql
SELECT MIN(salario) AS SalarioMinimo, MAX(salario) AS SalarioMaximo
FROM empleados;
```

Ejemplo con fechas:
```sql
SELECT MIN(fecha_contratacion) AS PrimerEmpleado, MAX(fecha_contratacion) AS UltimoEmpleado
FROM empleados;
```

Ejemplo con texto:
```sql
SELECT MIN(nombre) AS PrimerNombre, MAX(nombre) AS UltimoNombre
FROM empleados;
```
---
## Uso con `GROUP BY`
Se pueden utilizar en combinación con GROUP BY para obtener valores extremos por categoría:
```sql
SELECT departamento, MIN(salario) AS SalarioMinimo, MAX(salario) AS SalarioMaximo
FROM empleados
GROUP BY departamento;
```
## **Conclusión**
Las funciones `MIN()` y `MAX()` son útiles para encontrar los valores extremos dentro de conjuntos de datos y pueden aplicarse a diferentes tipos de datos en SQL​.
