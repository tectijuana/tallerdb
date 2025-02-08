# SELECT from Nobel Tutorial  
**Nombre de Alumno:** Fernando Andre Urrea Gonzalez  
**Número de Control:** C22211401  
# Problema 1:
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950;
```
**Explicación:**  
Este query selecciona el año `yr`, la categoría `subject` y el ganador `winner` del premio Nobel en el año 1950. Utiliza `WHERE yr = 1950` para filtrar solo las filas que corresponden a ese año.
# Problema 2
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
```
**Explicación:**  
Se selecciona solo el nombre del ganador `winner` del premio Nobel de Literatura en el año 1962. La condición `WHERE yr = 1962 AND subject = 'literature'` restringe los resultados a ese año y categoría.
# Problema 3
```sql
SELECT yr, subject
  FROM nobel
 WHERE winner = 'Albert Einstein'
```
**Explicación:**  
Este query selecciona el año `yr` y la categoría `subject` del premio Nobel para el ganador 'Albert Einstein'.  La cláusula `WHERE winner = 'Albert Einstein'` filtra los resultados para mostrar únicamente las filas donde el ganador sea Albert Einstein.

# Problema 4
```sql
SELECT winner
  FROM nobel
 WHERE subject = 'peace'
   AND yr > 1999
```
**Explicación:**  
Este query selecciona el nombre del ganador `winner` del premio Nobel de la Paz. La condición `WHERE subject = 'peace' AND yr > 1999` filtra los resultados para mostrar únicamente los ganadores del premio de la Paz después del año 1999 (es decir, a partir del año 2000).

# Problema 5
```sql
SELECT yr,subject,winner
  FROM nobel
 WHERE yr >=1980
   AND yr <=1989
   AND subject = 'literature'
```
**Explicación:**  
Este query selecciona el año `yr`, la categoría `subject` y el ganador `winner` del premio Nobel de Literatura entre los años 1980 y 1989 (ambos inclusive). La condición `WHERE yr >= 1980 AND yr <= 1989 AND subject = 'literature'`  filtra los resultados para ese rango de años y categoría específica.

# Problema 6
```sql
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Thomas Woodrow Wilson',
                  'Jimmy Carter',
                  'Barack Obama')
```
**Explicación:**  
Este query selecciona todas las columnas (`SELECT *`) de la tabla `nobel` para los ganadores que se encuentran en la lista especificada.  `WHERE winner IN (...)`  filtra los resultados para incluir únicamente las filas donde el ganador coincida con alguno de los nombres proporcionados.

# Problema 7
```sql
SELECT winner FROM nobel
 WHERE winner LIKE 'John%'
```
**Explicación:**  
Este query selecciona el nombre del ganador `winner` de la tabla `nobel` cuyo nombre comienza con "John". `WHERE winner LIKE 'John%'` utiliza el operador `LIKE` con el comodín `%` para buscar coincidencias.  `%` representa cualquier secuencia de caracteres (cero o más).

# Problema 8
```sql
SELECT * FROM nobel
 WHERE (yr = 1980 AND subject = 'physics')
    OR (yr = 1984 AND subject = 'chemistry')
```
**Explicación:**  
Este query selecciona todas las columnas (`SELECT *`) de la tabla `nobel` para las entradas que cumplen cualquiera de las dos condiciones especificadas.  La condición `(yr = 1980 AND subject = 'physics')` busca ganadores de física en 1980, mientras que `(yr = 1984 AND subject = 'chemistry')` busca ganadores de química en 1984. El operador `OR` combina estas dos condiciones.

# Problema 9
```sql
SELECT * FROM nobel
 WHERE subject NOT IN ('chemistry', 'medicine')
   AND yr = 1980
```
**Explicación:**  
Este query selecciona todas las columnas (`SELECT *`) de la tabla `nobel` para el año 1980, pero excluye a los ganadores de las categorías de química y medicina. `WHERE subject NOT IN ('chemistry', 'medicine')`  filtra los resultados para excluir esas dos categorías, y `AND yr = 1980`  restringe los resultados al año 1980.

# Problema 10
```sql
SELECT * FROM nobel
 WHERE (subject = 'Medicine' and yr < 1910)
    OR (subject = 'Literature' and yr >=2004)
```
**Explicación:**  
Este query selecciona todas las columnas (`SELECT *`) de la tabla `nobel` para las entradas que cumplen una de dos condiciones. La primera condición `(subject = 'Medicine' AND yr < 1910)`  busca ganadores de medicina antes de 1910. La segunda condición `(subject = 'Literature' AND yr >= 2004)` busca ganadores de literatura a partir del año 2004 (incluido).  El operador `OR` combina ambas condiciones.

# Problema 11
```sql
SELECT * FROM nobel
 WHERE winner = 'PETER GRÜNBERG'
```
**Explicación:**  
Este query selecciona todas las columnas (`SELECT *`) de la tabla `nobel` para el ganador llamado 'PETER GRÜNBERG'. La condición `WHERE winner = 'PETER GRÜNBERG'` filtra los resultados para mostrar únicamente la fila correspondiente a ese ganador.

# Problema 12
```sql
SELECT * FROM nobel
 WHERE winner = 'EUGENE O''NEILL'
```
**Explicación:**  
Este query selecciona todas las columnas (`SELECT *`) de la tabla `nobel` para el ganador llamado 'EUGENE O'NEILL'.  Es importante notar el uso de las comillas simples dobles (`''`) dentro del nombre para escapar la comilla simple que forma parte del apellido.  `WHERE winner = 'EUGENE O''NEILL'` filtra los resultados para ese ganador específico.

# Problema 13
```sql
SELECT winner, yr, subject FROM nobel
 WHERE winner LIKE 'Sir%'
 ORDER BY yr DESC, winner ASC
```
**Explicación:**  
Este query selecciona el nombre del ganador `winner`, el año `yr` y la categoría `subject` de la tabla `nobel` para todos los ganadores cuyo nombre comienza con "Sir".  `WHERE winner LIKE 'Sir%'` filtra los resultados usando el comodín `%`.  `ORDER BY yr DESC, winner ASC` ordena los resultados primero por año en orden descendente (`DESC`) y luego por nombre del ganador en orden ascendente (`ASC`).

# Problema 14
```sql
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY subject IN ('chemistry','physics'),subject, winner
```
**Explicación:**  
Este query selecciona el nombre del ganador `winner` y la categoría `subject` de la tabla `nobel` para el año 1984. `WHERE yr = 1984` filtra los resultados para ese año. Al ordenar con `ORDER BY subject IN ('chemistry','physics')`, SQL evalúa la expresión booleana y asigna `0` cuando el campo `subject` no es ni `chemistry` ni `physics`, y `1` cuando sí lo es. Dado que la ordenación por defecto es ascendente, los registros con valor `0` se muestran primero y los de valor `1` después, situando los ganadores de `chemistry` y `physics` al final. Luego, se ordena por `subject` y por `winner` dentro de cada grupo de resultados.
