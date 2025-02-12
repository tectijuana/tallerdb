#Esteves Peña Samuel 22210762 28Samuel28
# 🌍 Consultas SQL para la Base de Datos "World"

A continuación se presentan diversas consultas SQL que permiten extraer información relevante de una base de datos mundial.

## 1️⃣ Seleccionar Nombre, Continente y Población

```sql
SELECT name, continent, population 
FROM world;
SELECT name 
FROM world
WHERE population >= 200000000;

SELECT name, gdp / population AS per_capita_gdp
FROM world
WHERE population > 200000000;

SELECT name, population / 1000000 AS population_millions
FROM world
WHERE continent = 'South America';

SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');

SELECT name
FROM world
WHERE name LIKE '%United%';

SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000;

SELECT name, 
       ROUND(population / 1000000, 2) AS population_millions, 
       ROUND(gdp / 1000000000, 2) AS gdp_billions
FROM world
WHERE continent = 'South America';

SELECT name,
       ROUND(gdp / population, -3) AS per_capita_gdp
FROM world
WHERE gdp >= 1000000000000;

SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital);

SELECT name
FROM world
WHERE name LIKE '%a%' 
  AND name LIKE '%e%' 
  AND name LIKE '%i%' 
  AND name LIKE '%o%' 
  AND name LIKE '%u%' 
  AND name NOT LIKE '% %';
