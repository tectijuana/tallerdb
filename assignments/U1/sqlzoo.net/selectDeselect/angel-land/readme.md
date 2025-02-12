# ANGEL ELOY LANDGRAVE REZA - 20211798
## SELECT de SELECT

### 1. Más grande que Rusia
`SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Romania') 'code'`
### 2. Más rico que Reino Unido
`SELECT name FROM world
WHERE continent = "Europe"
AND (gdp/population)>(SELECT gdp/population FROM world
WHERE name = "United Kingdom")
`
### 3. Vecinos de Argentina y Australia
`SELECT name,continent FROM world
WHERE continent IN (SELECT DISTINCT continent FROM world WHERE name IN ('Argentina','Australia'))
ORDER BY name`

### 4. Entre Canada y Polonia
`SELECT name, population FROM world
WHERE population>(SELECT population FROM world WHERE name='United Kingdom')
AND population<(SELECT population FROM world WHERE name='Germany')`

### 5. Porcentajes de Alemania
`SELECT name,CONCAT(ROUND((population/(SELECT population FROM world WHERE name='Germany'))*100,0),'%') AS percentage
FROM world
WHERE continent = 'Europe'`

### 6. Más grande que toda Europa
`SELECT name FROM world
WHERE gdp>
   ALL(SELECT MAX(gdp) FROM world WHERE continent = 'Europe' AND gdp>0)`

### 7. El más grande en cada continente
`SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)`

### 8. El primer país del cada continente
`SELECT continent, name FROM world x
WHERE name=
   (SELECT MIN(name) FROM world y 
   WHERE y.continent=x.continent)`

### 9. Preguntas dificiles que no utilia tecnicas no cubiertas en secciones anteriores
`SELECT name, continent, population FROM world
WHERE continent IN (SELECT continent FROM world
   GROUP BY continent HAVING MAX(population) <= 25000000)`

### 10. Tres veces más grande
`SELECT name, continent FROM world x
   WHERE population > ALL
     (SELECT (population * 3) FROM world y
   WHERE y.continent = x.continent AND y.name != x.name)`
