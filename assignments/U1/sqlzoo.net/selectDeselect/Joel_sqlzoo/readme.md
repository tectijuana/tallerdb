# Trabajo #2 SQL Zoo
## Joel Cuevas Estrada - 22210298

## Ejercicio 1
````sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
````
## Ejercicio 2
````sql
SELECT name FROM world
WHERE continent = 'Europe' && gdp/population >
     (SELECT gdp/population FROM world
      WHERE name='United Kingdom')
````
## Ejercicio 3
````sql
SELECT name, continent FROM world
WHERE continent IN 
    (SELECT continent FROM world WHERE name = 'Argentina'
    UNION
    SELECT continent FROM world WHERE name = 'Australia')
ORDER BY name;
````
## Ejercicio 4
````sql
SELECT name, population FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name ='United Kingdom')
&& population <
     (SELECT population FROM world
      WHERE name ='Germany') 
````
## Ejercicio 5
````sql
SELECT name, CONCAT 
    (ROUND
       (
    (population / (SELECT population FROM world WHERE name = 'Germany') 
       ) * 100, 0), '%') AS percentage
FROM world
WHERE continent = 'Europe';
````
## Ejercicio 6
````sql
SELECT name FROM world
WHERE gdp > 
(
    SELECT MAX(gdp) FROM world WHERE continent = 'Europe'
)
AND gdp IS NOT NULL;
````
## Ejercicio 7
````sql
SELECT continent, name, area FROM world
WHERE area IN 
(
    (SELECT MAX(area) FROM world WHERE continent = 'Africa'),
    (SELECT MAX(area) FROM world WHERE continent = 'Asia'),
    (SELECT MAX(area) FROM world WHERE continent = 'Oceania'),
    (SELECT MAX(area) FROM world WHERE continent = 'South America'),
    (SELECT MAX(area) FROM world WHERE continent = 'North America'),
    (SELECT MAX(area) FROM world WHERE continent = 'Asia'),
    (SELECT MAX(area) FROM world WHERE continent = 'Caribbean'),
    (SELECT MAX(area) FROM world WHERE continent = 'Europe'),
    (SELECT MAX(area) FROM world WHERE continent = 'Eurasia')
)
````
## Ejercicio 8
````sql
SELECT continent, name FROM world
WHERE name = 
(
    SELECT MIN(name) 
    FROM world AS w2 
    WHERE w2.continent = world.continent
);
````
## Ejercicio 9
````sql
SELECT name, continent, population
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    GROUP BY continent
    HAVING MAX(population) <= 25000000
);
````
## Ejercicio 10
````sql
SELECT name, continent
FROM world
WHERE name IN ('Russia', 'Australia', 'Brazil');
````
