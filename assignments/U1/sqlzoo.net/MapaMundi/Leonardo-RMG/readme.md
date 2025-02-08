*Martinez Guzman Leonardo Rafael*
```sql
-- 1. Seleccionar nombre, continente y población de todos los países
SELECT name, continent, population FROM world;

-- 2. Seleccionar los países con población mayor o igual a 200 millones
SELECT name FROM world
WHERE population >= 200000000;

-- 3. Calcular el PIB per cápita para países con más de 200 millones de habitantes
SELECT name, gdp/population 
FROM world
WHERE population > 200000000;

-- 4. Obtener los países de Sudamérica con población en millones
SELECT name, population/1000000
FROM world
WHERE continent = "South America";

-- 5. Seleccionar Francia, Alemania e Italia
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');

-- 6. Buscar países con "United" en su nombre
SELECT name
FROM world
WHERE name LIKE '%United%';

-- 7. Países con área mayor a 3 millones o población mayor a 250 millones
SELECT name, population, area  
FROM world  
WHERE area > 3000000 OR population > 250000000;

-- 8. Países donde solo una condición (área o población) es verdadera
SELECT name, population, area  
FROM world  
WHERE (area > 3000000 XOR population > 250000000);

-- 9. Mostrar población en millones y PIB en miles de millones para Sudamérica
SELECT name, 
       ROUND(population / 1000000, 2) AS population_millions, 
       ROUND(gdp / 1000000000, 2) AS gdp_billions
FROM world
WHERE continent = 'South America';

-- 10. Redondear el PIB per cápita a la unidad de mil más cercana para países con PIB >= 1 billón
SELECT name,
       ROUND(gdp / population, -3) AS per_capita_gdp
FROM world
WHERE gdp >= 1000000000000;

-- 11. Países donde el nombre y la capital tienen la misma longitud
SELECT name, capital  
FROM world  
WHERE LENGTH(name) = LENGTH(capital);

-- 12. Países donde el nombre y la capital comienzan con la misma letra pero son diferentes
SELECT name, capital  
FROM world  
WHERE LEFT(name, 1) = LEFT(capital, 1)  
AND name <> capital;

-- 13. Países de una sola palabra que contienen todas las vocales
SELECT name  
FROM world  
WHERE name LIKE '%a%'  
AND name LIKE '%e%'  
AND name LIKE '%i%'  
AND name LIKE '%o%'  
AND name LIKE '%u%'  
AND name NOT LIKE '% %';
```

## Usage
