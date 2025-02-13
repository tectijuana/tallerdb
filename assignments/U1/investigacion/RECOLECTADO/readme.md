--------
Diaz Morales Katherine Giselle - 22210302
--------
Usa HAVING para filtrar sobre agregados.  
Filtra resultados después de aplicar funciones agregadas como SUM o AVG.  

----------

Lista las categorías de productos cuya suma total de ventas supera los 10,000.

```sql
SELECT categoria, SUM(ventas) AS total_ventas
FROM productos
GROUP BY categoria
HAVING SUM(ventas) > 10000;
````

- **GROUP BY** categoria agrupa los productos por categoría.
- **SUM(ventas)** calcula el total de ventas por categoría.
- **HAVING SUM(ventas)** > 10000 filtra solo las categorías con ventas totales mayores a 10,000.


----------------------------
### Diferencia entre WHERE y HAVING  


|Condición                                                          |Usar WHERE|Usar HAVING|
|-------------------------------------------------------------------|----------|-----------|
|Filtrar filas individuales antes de agrupar                        |  ✅ Sí  |  ❌ No    |
|Filtrar después de aplicar funciones agregadas (SUM(), AVG(), etc.)|  ❌ No  |    ✅ Sí  |  



#### Ejemplo de diferencia  
Este código **NO** funciona porque WHERE no puede filtrar un SUM():  


````sql

SELECT categoria, SUM(ventas)
FROM productos
WHERE SUM(ventas) > 10000  -- ❌ ERROR
GROUP BY categoria;
````


**CORRECCIÓN** con HAVING:  

````sql

SELECT categoria, SUM(ventas)
FROM productos
GROUP BY categoria
HAVING SUM(ventas) > 10000;
````
