# Actividad SQLzoo.net

## 1. Introduccion
SELECT name, continent, population FROM world

## 2. Paises Grandes
SELECT name FROM world
WHERE population >= 200000000

## 3. GDP Per Capita
SELECT name,gdp/population FROM world
WHERE population >= 200000000

## 4. Millones en Sudamérica
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'

## 5. Francia, Italia, Almeania
SELECT name, population FROM world
WHERE name = 'France' OR name = 'Italy' OR name = 'Germany'

## 6. Unidos
SELECT name FROM world
WHERE name LIKE 'United%'

## 7. Dos maneras para ser grande
SELECT name,population,area FROM world
WHERE area >=3000000 OR population >= 250000000

## 8. Es uno y otro (pero no ambos)
SELECT name,population,area FROM world
WHERE area >= 3000000 XOR population >= 250000000

## 9. Redondeo
SELECT name, ROUND(population/1000000,2),ROUND(gdp/1000000000.0,2) FROM world
WHERE continent = 'South America'

## 10. Economias trillonarias
SELECT name, ROUND(gdp/population, -3) AS per_capita_gdp
  FROM world
WHERE gdp > 1000000000000;

## 11. Nombre y capital tienen la misma longitud
SELECT name, capital
  FROM world
 WHERE LENGTH(name) = LENGTH(capital)

## 12. Ligando nombre y capital
SELECT name, capital
  FROM world
WHERE LEFT(name,1) = LEFT(capital,1) AND name<>capital

## 13. Todas las vocales
SELECT name
FROM world
WHERE name LIKE '%a% AND name LIKE '%e%' AND name LIKE '%i%' AND name LIKE '%o%' AND name LIKE '%u%'
AND name NOT LIKE '% %';
