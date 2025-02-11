# López Félix Nicolás

## SELECT within SELECT
## 1.- List each country name where the population is larger than that of 'Russia'.
```Sql
SELECT name FROM world
 WHERE population > (SELECT population FROM world
       WHERE name = 'Russia')
```

## 2.- Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```Sql
SELECT name FROM world
 WHERE gdp/population > (SELECT gdp/population FROM world
                          WHERE name = 'United Kingdom')
   AND continent = 'Europe'
```
## 3.- List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```Sql
SELECT name, continent FROM world  
 WHERE continent IN (SELECT continent FROM world 
                      WHERE name IN ('Argentina', 'Australia'))
Order by name
```
## 4.- Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
```Sql
SELECT name, population FROM world
 WHERE population > (SELECT population FROM world
                      WHERE name = 'United Kingdom')
   AND population < (SELECT population FROM world
                            WHERE name = 'Germany')
```
## 5.- Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```Sql
SELECT name, CONCAT(ROUND(population / (SELECT population FROM world WHERE name = 'Germany') * 100, 0), '%') percentage FROM world
 WHERE continent = 'Europe'
```
## 6.- Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values).
```Sql
SELECT name FROM world
 WHERE gdp > ALL(SELECT gdp FROM world
 WHERE gdp > 0 AND continent = 'Europe')
```
## 7.- Find the largest country (by area) in each continent, show the continent, the name and the area:
```Sql
SELECT continent, name, area FROM world x
 WHERE area >= ALL (SELECT area FROM world y
 WHERE y.continent = x.continent AND area > 0)
```
## 8.- List each continent and the name of the country that comes first alphabetically.
```Sql
SELECT continent, name FROM (SELECT continent, name,
       ROW_NUMBER() OVER (PARTITION BY continent
 ORDER BY name ASC) AS rn FROM world ) ranked
 WHERE rn = 1
```
## 9.- Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```Sql
SELECT name, continent, population FROM world
 WHERE continent IN (SELECT continent FROM world
 GROUP BY continent HAVING MAX(population) <= 25000000)
```
## 10.- Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.
```Sql
SELECT DISTINCT x.name, x.continent FROM world x
  JOIN world y ON x.continent = y.continent
   AND x.name != y.name
 WHERE x.population > 3 * (SELECT MAX(z.population) FROM world z
 WHERE z.continent = x.continent
   AND z.name != x.name)
```
