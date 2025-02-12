# Elaborado por Emmanuel Del Angel - 21210366
### Actividad - Práctica de SQL con la herramienta de apoyo SQLZoo: Problemas tutorial select dentro del select
- Este tutorial analiza cómo podemos utilizar declaraciones SELECT dentro de declaraciones SELECT para realizar consultas más complejas.

``` sql
(name, continent, area, population, gdp)
```
### Problemas del 1 al 10
** Problema 1: Más grande que Rusia **
``` sql
# Problema 1: Enumere el nombre de cada país cuya población sea mayor que la de Rusia.
SELECT name FROM world
WHERE population >
 (SELECT population FROM world
WHERE name='Russia');
```

**Problema 2: Más rico que el Reino Unido**
``` sql
# Problema 2: Mostrar los países de Europa con un PIB per cápita mayor que 'Reino Unido'.
SELECT name  
FROM world  
WHERE continent = 'Europe'  
AND gdp / population > (SELECT gdp / population FROM world WHERE name = 'United Kingdom');
```
**Problema 3: Vecinos de Argentina y Australia**
``` sql
# Problema 3: Enumere el nombre y el continente de los países que contienen a Argentina o Australia . Ordene por nombre del país..
SELECT name, continent  
FROM world  
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))  
ORDER BY name;
```
**Problema 4: Entre Canadá y Polonia**
``` sql
# Problema 4: ¿Qué país tiene una población mayor que la del Reino Unido pero menor que la de Alemania? Indica el nombre y la población.
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```
**Problema 5: Porcentajes de Alemania**
``` sql
# Problema 5: Muestra el nombre y la población de cada país de Europa. Muestra la población como porcentaje de la población de Alemania..
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```
**Problema 6: Más grande que todos los países de Europa**
``` sql
# Problema 6: ¿Qué países tienen un PIB mayor que todos los países de Europa? [Indique sólo el nombre .] (Algunos países pueden tener valores de PIB nulos).
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```
**Problema 7: Más grande de cada continente**
``` sql
# Problema 7: Encuentra el país más grande (por área) en cada continente, muestra el continente , el nombre y el área :.
SELECT continent, name, area  
FROM world w1  
WHERE area = (SELECT MAX(area) FROM world w2 WHERE w1.continent = w2.continent);
```
**Problema 8: Primer país de cada continente (en orden alfabético)**
``` sql
# Problema 8: Enumere cada continente y el nombre del país que aparece primero en orden alfabético..
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent);
```
**Problema 9: Preguntas difíciles que utilizan técnicas no cubiertas en secciones anteriores**
- Encuentra los continentes donde todos los países tienen una población <= 25000000. Luego encuentra los nombres de los países asociados con estos continentes. Muestra el nombre , el continente y la población .
``` sql
# Problema 9
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000);
```
**Problema 10: Tres veces más grande**
- Algunos países tienen una población tres veces mayor que la de todos sus vecinos (en el mismo continente). Mencione los países y los continentes.
``` sql
# Problema 10:
SELECT name, continent
FROM world w1
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world w2
    WHERE w1.continent = w2.continent
    AND w2.name != w1.name
);
```
