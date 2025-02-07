1. SELECT name, continent, population FROM world
2. SELECT name FROM world
WHERE population >= 200000000
3. SELECT name,gdp/population FROM world
WHERE population >= 200000000
4. SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
5. SELECT name, population FROM world
WHERE name = 'France' OR name = 'Italy' OR name = 'Germany'
6. SELECT name FROM world
WHERE name LIKE 'United%'
7. SELECT name,population,area FROM world
WHERE area >=3000000 OR population >= 250000000
8. SELECT name,population,area FROM world
WHERE area >= 3000000 XOR population >= 250000000
9. SELECT name, ROUND(population/1000000,2),ROUND(gdp/1000000000.0,2) FROM world
WHERE continent = 'South America'
