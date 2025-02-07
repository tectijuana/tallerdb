# SQLZoo Mapamundi
## Joel Cuevas Estrada - 22210298
### Ejercicios

- Problema #1
  
```sql
SELECT name, continent, population FROM world

```
- Problema #2
```sql
SELECT name FROM world
WHERE population >= 200000000
```
- Problema #3
```sql
SELECT name, gdp/population FROM world
WHERE population >= 200000000
```
- Problema #4
```sql
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
```
- Problema #5
```sql
SELECT name, population FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```
- Problema #6
```sql
SELECT name FROM world
WHERE name LIKE '%United%';
```
- Problema #7
```sql
SELECT name, population, area FROM world
WHERE population >= 250000000 OR area >= 3000000;
```
- Problema #8
```sql
SELECT name, population, area FROM world
WHERE area >= 3000000 XOR population >= 250000000; 
```
- Problema #9
```sql
SELECT name, 
       ROUND(population / 1000000, 2), 
       ROUND (gdp / 1000000000, 2) 
FROM world WHERE continent = 'South America';
```
- Problema #10
```sql
SELECT name, gdp/population FROM world
WHERE (gdp/population, -3)
```
- Problema #11
```sql
SELECT name, capital
  FROM world
 WHERE LENGTH(name) =  LENGTH(capital)
```
- Problema #12
```sql
SELECT name, capital
FROM world
WHERE Left(name,1) = Left(capital,1) && name <> capital
```
- Problema #13
```sql
SELECT name 
FROM world
WHERE name LIKE '%a%'
AND name LIKE '%e%'
AND name LIKE '%i%'
AND name LIKE '%o%'
AND name LIKE '%u%'
AND name NOT LIKE '% %';
```
