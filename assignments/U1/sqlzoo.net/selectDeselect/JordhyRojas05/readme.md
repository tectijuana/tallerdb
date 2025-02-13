# SELECT within SELECT Tutorial (Jordhy Rojas 22210349)
This tutorial looks at how we can use SELECT statements within SELECT statements to perform more complex queries.

![image](https://github.com/user-attachments/assets/d6a78ba9-4de8-44db-9129-b0ce513cb694)

## 1.	Bigger than Russia
🌐 List each country name where the population is larger than that of 'Russia'.
```sql
SELECT name 
FROM world
WHERE population > 146000000
```

## 2.	Richer than UK
🌐 Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
SELECT name  
FROM world  
WHERE continent = 'Europe'  
AND gdp / population > (SELECT gdp / population FROM world WHERE name = 'United Kingdom');
```

## 3.	Neighbours of Argentina and Australia
🌐 List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent  
FROM world  
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))  
ORDER BY name;
```

## 4.	Between Canada and Poland
🌐 Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
```sql
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```

## 5.	Percentages of Germany
🌐 Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.

Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

The format should be Name, Percentage for example:

![image](https://github.com/user-attachments/assets/c3aa1a33-e969-4aba-8b23-b544608f523f)
```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```

## 6.	Bigger than every country in Europe
🌐 Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```
We can refer to values in the outer SELECT within the inner SELECT. We can name the tables so that we can tell the difference between the inner and outer versions.

## 7.	Largest in each continent
🌐 Find the largest country (by area) in each continent, show the continent, the name and the area:

The above example is known as a correlated or synchronized sub-query.
```sql
SELECT continent, name, area  
FROM world A 
WHERE area = (SELECT MAX(area) FROM world B WHERE A.continent = B.continent);
```

## 8.	First country of each continent (alphabetically)
🌐 List each continent and the name of the country that comes first alphabetically.

```sql
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent);
```

## 9.	Difficult Questions That Utilize Techniques Not Covered In Prior Sections
🌐 Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000);
```

## 10.	Three time bigger
🌐 Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT name, continent
FROM world A
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world B
    WHERE A.continent = B.continent
    AND B.name != A.name
);
```
