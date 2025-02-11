# Perez Lopez Oscar Ricardo 23210645

## 1. List each country name where the population is larger than that of 'Russia'. 
`world(name, continent, area, population, gdp)`

```sql
SELECT name  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'Russia');
```

## 2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

```sql
SELECT name  
FROM world  
WHERE continent = 'Europe'  
AND (gdp / population) > (SELECT gdp / population FROM world WHERE name = 'United Kingdom');
```

## 3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

```sql
SELECT name, continent  
FROM world  
WHERE continent IN (
    SELECT continent FROM world WHERE name IN ('Argentina', 'Australia')
)  
ORDER BY name;
```

## 4. Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.

```sql
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```

## 5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.  
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```

## 6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)

```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe')  
AND gdp IS NOT NULL;
```

## 7. Find the largest country (by area) in each continent, show the continent, the name and the area:  
The above example is known as a correlated or synchronized sub-query.

```sql
SELECT continent, name, area  
FROM world  
WHERE area = (SELECT MAX(area) FROM world AS w2 WHERE w2.continent = world.continent);
```

## 8. List each continent and the name of the country that comes first alphabetically.

```sql
SELECT continent, name  
FROM world AS w1  
WHERE name = (SELECT MIN(name) FROM world AS w2 WHERE w2.continent = w1.continent);
```

## 9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

```sql
SELECT name, continent, population  
FROM world  
WHERE continent IN (
    SELECT continent  
    FROM world  
    GROUP BY continent  
    HAVING MAX(population) <= 25000000
);
```

## 10. Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.

```sql
SELECT w1.name, w1.continent  
FROM world w1  
WHERE NOT EXISTS (
    SELECT 1  
    FROM world w2  
    WHERE w2.continent = w1.continent  
    AND w2.name != w1.name  
    AND w1.population <= 3 * w2.population
);
```
```
