# 💡 Calcula fechas con DATEDIFF y DATEADD  
Resta o suma días entre fechas para análisis temporal.  

## 📆 ¿Qué son DATEDIFF y DATEADD?  
- **DATEDIFF**: Calcula la diferencia entre dos fechas en una unidad específica (días, meses, años, etc.).  
- **DATEADD**: Agrega o resta una cantidad específica de unidades de tiempo a una fecha.  

## 📊 Ejemplo: Analizar cambios de fechas  
Imagina que queremos:  
- Calcular la antigüedad de los empleados en una empresa.  
- Determinar quiénes han trabajado más de 5 años.  

### ❌ Sin DATEDIFF y DATEADD (Consulta compleja y difícil de leer):  
```sql
SELECT empleado_id, fecha_ingreso, 
       CASE 
           WHEN (YEAR(GETDATE()) - YEAR(fecha_ingreso)) > 5 THEN 'Más de 5 años'
           ELSE 'Menos de 5 años'
       END AS tiempo_en_empresa
FROM empleados;

```
#### 🔍 Problema: Se requiere lógica adicional para manejar fechas exactas.

### ✅ Con DATEDIFF y DATEADD (Consulta más clara y ordenada):
```sql
WITH Antiguedad AS (
    SELECT empleado_id, fecha_ingreso, 
           DATEDIFF(YEAR, fecha_ingreso, GETDATE()) AS años_trabajados
    FROM empleados
),
FiltroAntiguedad AS (
    SELECT empleado_id, fecha_ingreso, años_trabajados
    FROM Antiguedad
    WHERE fecha_ingreso <= DATEADD(YEAR, -5, GETDATE())
)
SELECT empleado_id, fecha_ingreso, años_trabajados
FROM FiltroAntiguedad;
```

### ✅ Ventajas de usar DATEDIFF y DATEADD con CTEs
- ✔ Legibilidad: Divide la consulta en partes lógicas y claras.
- ✔ Reutilización: Puedes usar la misma CTE en diferentes consultas.
- ✔ Depuración: Es más fácil probar cada parte del análisis.

### 💡 Tip Extra
- También puedes usar DATEADD para calcular fechas futuras, como vencimientos de contratos o proyecciones de ventas.

#### Elaborado por: Meza Rodriguez Eduardo Manuel #21211997
