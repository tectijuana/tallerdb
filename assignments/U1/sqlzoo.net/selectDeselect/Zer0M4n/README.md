# 22211575 CESAR GONZALEZ SALAZAR

## 1.List each country name where the population is larger than that of 'Russia'.

```sql
world(name, continent, area, population, gdp)
```
SOLUCION
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

## 2.Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

SOLUCION
```sql
SELECT name FROM world
WHERE continent = 'Europe' 
AND gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom')
```

## 3.List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
SOLUCION
```sql
SELECT name, continent FROM world
WHERE continent IN (SELECT continent FROM world WHERE name in ('Argentina',  'Australia'))
ORDER BY name
```


## 4.Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
SOLUCION
```sql
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```


## 5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

<table class="sqlmine"><tbody><tr><th>name</th><th>percentage</th></tr><tr><td>Albania</td><td>3%</td></tr><tr><td>Andorra</td><td>0%</td></tr><tr><td>Austria</td><td>11%</td></tr><tr><td>...</td><td>...</td></tr></tbody></table>

SOLUCION
```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```


## 6.Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
SOLUCION
```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```

## 7. Find the largest country (by area) in each continent, show the continent, the name and the area: The above example is known as a correlated or synchronized sub-query.

SOLUCION
```sql
SELECT continent, name, area  
FROM world w1  
WHERE area = (SELECT MAX(area) FROM world w2 WHERE w1.continent = w2.continent);

```

## 8.List each continent and the name of the country that comes first alphabetically.

SOLUCION
```sql
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent)

```


## 9.Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population

SOLUCION
```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000
)
```


## 10.Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continent

SOLUCION
```sql
SELECT name, continent
FROM world x
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world y
    WHERE x.continent = y.continent
    AND y.name != x.name);
```
