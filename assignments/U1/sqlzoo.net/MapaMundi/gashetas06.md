# Morales Hernandez Christian Gahel
# Consultas SQL para la base de datos `world`

# 1. Seleccionar el nombre, continente y población de todos los países
SELECT name, continent, population 
FROM world;

# 2. Seleccionar los países con una población mayor o igual a 200 millones
SELECT name FROM world
WHERE population >= 200000000;

# 3. Calcular el PIB per cápita para países con población mayor a 200 millones
SELECT name, gdp / population 
FROM world
WHERE population > 200000000;

# 4. Convertir la población de Sudamérica a millones
SELECT name, population / 1000000
FROM world
WHERE continent = "South America";

# 5. Seleccionar Francia, Alemania e Italia
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');

# 6. Seleccionar países con "United" en el nombre
SELECT name
FROM world
WHERE name LIKE '%United%';

# 7. Seleccionar países con área mayor a 3 millones de km² o población mayor a 250 millones
SELECT name, population, area  
FROM world  
WHERE area > 3000000 OR population > 250000000;

# 8. Seleccionar países con área mayor a 3 millones de km² o población mayor a 250 millones, pero no ambos
SELECT name, population, area  
FROM world  
WHERE (area > 3000000 XOR population > 250000000);

# 9. Redondear población y PIB de Sudamérica
SELECT name, 
       ROUND(population / 1000000, 2) AS population_millions, 
       ROUND(gdp / 1000000000, 2) AS gdp_billions
FROM world
WHERE continent = 'South America';

# 10. Calcular PIB per cápita redondeado a miles para países con PIB mayor o igual a un billón
SELECT name,
       ROUND(gdp / population, -3) AS per_capita_gdp
FROM world
WHERE gdp >= 1000000000000;

# 11. Seleccionar países donde el nombre y la capital tienen la misma longitud
SELECT name, capital  
FROM world  
WHERE LENGTH(name) = LENGTH(capital);

# 12. Seleccionar países cuya capital comienza con la misma letra que el nombre del país (excluyendo coincidencias exactas)
SELECT name, capital  
FROM world  
WHERE LEFT(name, 1) = LEFT(capital, 1)  
AND name <> capital;

# 13. Seleccionar países cuyos nombres contienen todas las vocales y no tienen espacios
SELECT name  
FROM world  
WHERE name LIKE '%a%'  
AND name LIKE '%e%'  
AND name LIKE '%i%'  
AND name LIKE '%o%'  
AND name LIKE '%u%'  
AND name NOT LIKE '% %';
