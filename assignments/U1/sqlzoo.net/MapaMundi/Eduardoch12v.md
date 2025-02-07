*Cruz Hernandez Edgar Eduardo*

```sql
SELECT name, continent, population FROM world;

SELECT name FROM world
WHERE population >= 200000000;

SELECT name, gdp/population
FROM world
WHERE population > 200000000;

SELECT name, population/1000000
FROM world
WHERE continent = "South America";

SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');

SELECT name
FROM world
WHERE name LIKE '%United%';

SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000;

SELECT name, population, area
FROM world
WHERE (area > 3000000 AND population <= 250000000)
   OR (population > 250000000 AND area <= 3000000);

SELECT name,
       ROUND(population / 1000000.0, 2) AS population_in_millions,
       ROUND(gdp / 1000000000.0, 2) AS gdp_in_billions
FROM world
WHERE continent = 'South America';

SELECT name,
       ROUND(gdp / population, -3) AS per_capita_gdp
FROM world
WHERE gdp >= 1000000000000;

SELECT name, capital  
FROM world  
WHERE LENGTH(name) = LENGTH(capital);

SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1)
AND name <> capital;
 ```
