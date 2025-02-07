*Cruz Hernandez Edgar Eduardo 22210296*

```sql
-- #Problema 1
SELECT name, continent, population FROM world;

-- #Problema 2
SELECT name FROM world
WHERE population >= 200000000;

-- #Problema 3
SELECT name, gdp/population
FROM world
WHERE population > 200000000;

-- #Problema 4
SELECT name, population/1000000
FROM world
WHERE continent = "South America";

-- #Problema 5
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');

-- #Problema 6
SELECT name
FROM world
WHERE name LIKE '%United%';

-- #Problema 7
SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000;

-- #Problema 8
SELECT name, population, area
FROM world
WHERE (area > 3000000 AND population <= 250000000)
   OR (population > 250000000 AND area <= 3000000);

-- #Problema 9
SELECT name,
       ROUND(population / 1000000.0, 2) AS population_in_millions,
       ROUND(gdp / 1000000000.0, 2) AS gdp_in_billions
FROM world
WHERE continent = 'South America';

-- #Problema 10
SELECT name,
       ROUND(gdp / population, -3) AS per_capita_gdp
FROM world
WHERE gdp >= 1000000000000;

-- #Problema 11
SELECT name, capital  
FROM world  
WHERE LENGTH(name) = LENGTH(capital);

-- #Problema 12
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1)
AND name <> capital;

-- #Problema 13
SELECT name
FROM world
WHERE name LIKE '%a%'
AND name LIKE '%e%'
AND name LIKE '%i%'
AND name LIKE '%o%'
AND name LIKE '%u%'
AND name NOT LIKE '% %';
```

