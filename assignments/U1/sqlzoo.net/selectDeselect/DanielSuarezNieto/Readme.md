# SQL SELECT  
## Nombre: Luis Daniel Suarez Nieto - 22210356
## Ejercicios  

Este ejercicio explora el uso de subconsultas en SQL para realizar consultas más avanzadas en una base de datos que contiene información sobre países del mundo. Se enfoca en la selección de datos basada en comparaciones con valores obtenidos dinámicamente mediante subconsultas dentro de la cláusula WHERE.

A lo largo del ejercicio, se aplican conceptos clave como:

Comparaciones con valores específicos obtenidos de subconsultas (por ejemplo, países con mayor población que Rusia).
Uso de operadores como ALL e IN para comparar un valor con múltiples resultados de una subconsulta.
Filtrado basado en cálculos dentro de la consulta (como el PIB per cápita o la población relativa a otro país).
Subconsultas correlacionadas, que dependen de valores en la consulta principal, y no correlacionadas, que pueden ejecutarse por separado.
Estos ejercicios permiten comprender cómo SQL puede trabajar con datos dinámicos, comparar valores en diferentes niveles y extraer información más específica mediante subconsultas. 💡

### 1  
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```
### 2  
```sql
SELECT name FROM world
WHERE continent='Europe' AND gdp/population >
 (SELECT gdp/population FROM world
 WHERE name='United Kingdom');
```
### 3  
```sql
SELECT name, continent FROM world
WHERE continent IN (
SELECT continent FROM world
WHERE name='Argentina' OR name='Australia'
)
ORDER BY name ASC;
```
### 4  
```sql
SELECT name, population FROM world
WHERE population > (
SELECT population FROM world
WHERE name='Canada'
) AND population< (
SELECT population FROM world
WHERE name='Poland'
);
```
### 5  
```sql
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name='Germany')*100, 0),'%')
FROM world
WHERE continent='Europe';
```

### 6  
```sql
SELECT name 
FROM world
WHERE gdp > ALL(SELECT gdp 
FROM world
WHERE continent='Europe'
AND gdp IS NOT NULL);
```
### 7 
```sql
SELECT continent, name, area FROM world x
WHERE area >= ALL
(SELECT area FROM world y
WHERE y.continent=x.continent
AND area>0);
```
### 8  
```sql
SELECT continent, name FROM world x 
WHERE name <= ALL
(SELECT name FROM world y
WHERE x.continent=y.continent
AND name IS NOT NULL);
```
### 9  
```sql
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(SELECT population FROM world y WHERE x.continent=y.continent);
```
### 10  
```sql
SELECT name, continent
FROM world x
WHERE population > ALL(
SELECT population*3 FROM world y 
WHERE y.continent=x.continent
AND y.name!=x.name);
```
