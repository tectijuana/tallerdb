# Brian Guadalupe Tellez Escobar C21211400 10/02/2025
------------------------------------------------------------------------------------

## 1. Países con una población mayor que la de Rusia

### Descripción
Lista los nombres de los países cuya población es mayor que la de 'Rusia'.

```sql
SELECT name 
FROM world
WHERE population > (
  SELECT population 
  FROM world
  WHERE name = 'Russia'
);
```
## 2. Países más ricos que el Reino Unido (por PIB per cápita)
### Descripción
Muestra los países de Europa cuyo PIB per cápita es mayor que el del 'United Kingdom'.

```sql
SELECT name
FROM world
WHERE continent = 'Europe'
  AND gdp/population > (
    SELECT gdp/population 
    FROM world 
    WHERE name = 'United Kingdom'
);
```
## 3. Vecinos de Argentina y Australia
### Descripción
Lista el nombre y el continente de los países ubicados en los continentes que contienen a 'Argentina' o 'Australia'. Los resultados se ordenan alfabéticamente por nombre del país.

```sql
SELECT name, continent 
FROM world
WHERE continent IN (
  SELECT continent 
  FROM world
  WHERE name IN ('Argentina', 'Australia')
)
ORDER BY name ASC;
```
## 4. Entre Canadá y Polonia
### Descripción
Encuentra el país cuya población es mayor que la del 'United Kingdom' pero menor que la de 'Germany'. Muestra el nombre del país y su población.

```sql
SELECT name, population 
FROM world
WHERE population > (
    SELECT population 
    FROM world 
    WHERE name = 'United Kingdom'
  )
  AND population < (
    SELECT population 
    FROM world 
    WHERE name = 'Germany'
);
```
## 5. Porcentajes respecto a Alemania
### Descripción
Muestra el nombre y la población de cada país de Europa como un porcentaje de la población de Alemania (80 millones).

```sql
SELECT name, CONCAT(
  ROUND(population / (
    SELECT population 
    FROM world 
    WHERE name = 'Germany'
  ) * 100, 0), '%'
) AS percentage
FROM world
WHERE continent = 'Europe';
```
## 6. Países más grandes que todos los países de Europa (por PIB)
### Descripción
Muestra los países cuyo PIB es mayor que el de todos los países de Europa.

```sql
SELECT name 
FROM world
WHERE gdp > ALL (
  SELECT gdp 
  FROM world
  WHERE gdp > 0 AND continent = 'Europe'
);
```
## 7. País más grande por continente (por área)
### Descripción
Encuentra el país más grande por área en cada continente. Muestra el continente, el nombre del país y el área.

```sql
SELECT continent, name, area 
FROM world x
WHERE area >= ALL (
  SELECT area 
  FROM world y
  WHERE y.continent = x.continent
    AND area > 0
);
```
## 8. Primer país por continente (alfabéticamente)
### Descripción
Lista cada continente y el nombre del país que aparece primero en orden alfabético.

```sql
SELECT continent, name 
FROM world x
WHERE name <= ALL (
  SELECT name 
  FROM world y
  WHERE y.continent = x.continent
);
```
## 9. Continentes con países de población menor o igual a 25,000,000
### Descripción
Encuentra los continentes donde todos los países tienen una población menor o igual a 25,000,000. Luego, muestra el nombre, el continente y la población de los países en dichos continentes.

```sql
SELECT name, continent, population 
FROM world x
WHERE 25000000 >= ALL (
  SELECT population 
  FROM world y
  WHERE x.continent = y.continent
    AND y.population > 0
);
```
## 10. Países con una población tres veces mayor que la de sus vecinos
### Descripción
Muestra los países cuya población es al menos tres veces mayor que la de cualquier otro país en el mismo continente.

```sql
SELECT name, continent 
FROM world x
WHERE population >= ALL (
  SELECT population * 3 
  FROM world y
  WHERE x.continent = y.continent
    AND y.name != x.name
);
```
