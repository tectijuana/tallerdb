#Valenzuela Soto David Bernabe 
#23210677 

## Usa `LAG()` y `LEAD()` para comparar filas consecutivas

### Introducción
Las funciones `LAG()` y `LEAD()` en SQL permiten acceder a valores de filas anteriores o siguientes dentro de una misma partición de datos, facilitando el análisis comparativo de registros consecutivos.

### Sintaxis

```sql
SELECT columna1, columna2,
       LAG(columna1, 1, 0) OVER (PARTITION BY grupo ORDER BY orden) AS valor_anterior,
       LEAD(columna1, 1, 0) OVER (PARTITION BY grupo ORDER BY orden) AS valor_siguiente
FROM tabla;
```

- `LAG(columna, desplazamiento, valor_predeterminado) OVER (...)`: Recupera el valor de la fila anterior.
- `LEAD(columna, desplazamiento, valor_predeterminado) OVER (...)`: Recupera el valor de la fila siguiente.
- `PARTITION BY`: Divide el conjunto de datos en grupos.
- `ORDER BY`: Especifica el orden dentro de cada grupo.

### Ejemplo

```sql
SELECT SalesYear, SalesTotal,
       LAG(SalesTotal, 1, 0) OVER (PARTITION BY SalesYear ORDER BY SalesTotal) AS Ventas_Anterior,
       LEAD(SalesTotal, 1, 0) OVER (PARTITION BY SalesYear ORDER BY SalesTotal) AS Ventas_Siguiente
FROM Ventas;
```

**Salida esperada**:

| SalesYear | SalesTotal | Ventas_Anterior | Ventas_Siguiente |
|-----------|-----------|-----------------|------------------|
| 2018      | 923746.85 | 0.00            | 974853.81        |
| 2018      | 974853.81 | 923746.85       | 1132744.56       |
| 2018      | 1132744.56| 974853.81       | NULL             |

### Uso práctico
- Comparación de ventas año a año.
- Identificación de tendencias ascendentes o descendentes en datos secuenciales.
- Cálculo de diferencias entre registros consecutivos.

 

---

## Implementa `GROUPING SETS` para resúmenes avanzados

### Introducción
La cláusula `GROUPING SETS` en SQL permite definir múltiples niveles de agrupamiento en una sola consulta, generando diferentes subconjuntos de agregaciones.

### Sintaxis

```sql
SELECT columna1, columna2, COUNT(*) AS total
FROM tabla
GROUP BY GROUPING SETS (
    (columna1), 
    (columna2), 
    (columna1, columna2),
    ()
);
```

- Cada conjunto dentro de `GROUPING SETS` define un nivel de agregación.
- Un conjunto vacío `()` genera un resumen total.

### Ejemplo

```sql
SELECT VendorState, VendorCity, COUNT(*) AS QtyVendors
FROM Vendors
WHERE VendorState IN ('IA', 'NJ')
GROUP BY GROUPING SETS (
    (VendorState, VendorCity), 
    (VendorState), 
    (VendorCity), 
    ()
)
ORDER BY VendorState DESC, VendorCity DESC;
```

**Salida esperada**:

| VendorState | VendorCity      | QtyVendors |
|-------------|----------------|------------|
| NJ         | Fairfield       | 2          |
| NJ         | East Brunswick  | 2          |
| NJ         | NULL            | 4          |
| IA         | Washington      | 2          |
| IA         | Fairfield       | 2          |
| IA         | NULL            | 4          |
| NULL       | NULL            | 8          |

### Beneficios
- Permite múltiples niveles de agregación en una sola consulta.
- Reduce la necesidad de usar múltiples consultas `UNION ALL`.
- Mejora la eficiencia y flexibilidad en reportes resumidos.

 

---

Estos formatos incluyen código, ejemplos prácticos y explicaciones claras, ideales para documentar estas funciones avanzadas en SQL.
