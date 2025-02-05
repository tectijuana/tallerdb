#Valderrama Dominguez Roberto - 23210674

## 📌 Implementa funciones de ventana  

### 📝 Descripción  
Las funciones de ventana en SQL permiten realizar cálculos sobre un conjunto de filas relacionadas dentro de una consulta sin afectar la agrupación general de los resultados. A diferencia de las funciones de agregación tradicionales, las funciones de ventana mantienen la visibilidad de cada fila en el conjunto de resultados.  

### 🔍 Características clave  
- **PARTITION BY**: Divide el conjunto de resultados en particiones para realizar cálculos independientes.  
- **ORDER BY**: Define el orden en el que se aplican las funciones de ventana.  
- **FRAME CLAUSE**: Especifica el rango de filas sobre el cual opera la función (ej. `ROWS BETWEEN` o `RANGE BETWEEN`).  

### 📌 Ejemplos  

#### ✅ Uso de `ROW_NUMBER()`
```sql
SELECT 
    id_empleado,
    nombre,
    departamento,
    ROW_NUMBER() OVER (PARTITION BY departamento ORDER BY id_empleado) AS num_fila
FROM empleados;
```
📝 Asigna un número de fila secuencial a cada empleado dentro de su departamento.  

#### ✅ Uso de `RANK()`
```sql
SELECT 
    id_empleado,
    nombre,
    salario,
    RANK() OVER (ORDER BY salario DESC) AS ranking_salario
FROM empleados;
```
📝 Asigna un ranking basado en el salario, permitiendo empates.  

#### ✅ Uso de `SUM()` con ventana  
```sql
SELECT 
    id_pedido,
    cliente,
    monto,
    SUM(monto) OVER (PARTITION BY cliente ORDER BY id_pedido) AS total_acumulado
FROM ventas;
```
📝 Calcula un total acumulado de compras por cliente.  

### 🏆 Beneficios  
✅ Mejora el rendimiento en comparación con subconsultas.  
✅ Facilita cálculos como acumulados, promedios móviles y rankings.  
✅ Permite realizar análisis avanzados sin perder visibilidad de los datos.  

---

## 📌 Optimiza consultas con índices  

### 📝 Descripción  
Los índices mejoran la velocidad de las consultas al permitir que la base de datos acceda a los datos de manera eficiente. Son estructuras auxiliares que evitan realizar escaneos completos de tablas.  

### 🔍 Tipos de índices  
- **Índice Clúster** (`CLUSTERED INDEX`) 📌: Ordena físicamente los datos en disco.  
- **Índice No Clúster** (`NONCLUSTERED INDEX`) 📌: Mantiene una estructura separada con referencias a la tabla.  
- **Índice Único** (`UNIQUE INDEX`) 📌: Evita valores duplicados en una columna.  
- **Índice Compuesto** 📌: Cubre múltiples columnas para optimizar búsquedas combinadas.  

### 📌 Ejemplos  

#### ✅ Crear un índice en una columna  
```sql
CREATE INDEX idx_cliente ON ventas(cliente);
```
📝 Acelera consultas que filtran por `cliente`.  

#### ✅ Crear un índice compuesto  
```sql
CREATE INDEX idx_cliente_fecha ON ventas(cliente, fecha);
```
📝 Optimiza consultas que filtran por `cliente` y `fecha`.  

#### ✅ Uso de `EXPLAIN` para analizar consultas  
```sql
EXPLAIN SELECT * FROM ventas WHERE cliente = 'Juan Perez';
```
📝 Muestra el plan de ejecución y permite identificar oportunidades de optimización.  

### 🏆 Buenas prácticas  
✅ **Usa índices en columnas con alta cardinalidad** (muchos valores únicos).  
✅ **Evita crear demasiados índices**, ya que pueden afectar el rendimiento en inserciones.  
✅ **Usa `COVERING INDEX`** para consultas frecuentes que recuperan datos de varias columnas.  
✅ **Mide el impacto con `EXPLAIN ANALYZE`** antes y después de aplicar índices.  

---

📌 **Conclusión:**  
- Las **funciones de ventana** son clave para análisis avanzados sin perder granularidad.  
- Los **índices** mejoran la eficiencia de las consultas, pero deben usarse estratégicamente.  

¡Optimiza tu SQL y lleva tu rendimiento al siguiente nivel! 🚀
