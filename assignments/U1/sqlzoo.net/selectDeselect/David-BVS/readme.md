# DAVID BERNABE VALENZUELA SOTO 
# 23210677
# 10/02/2025

## 1. Países con mayor población que Rusia
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```
## 2. Países de Europa con mayor PIB per cápita que el Reino Unido
```sql
SELECT name  
FROM world  
WHERE continent = 'Europe'  
AND gdp / population > (SELECT gdp / population FROM world WHERE name = 'United Kingdom');
```
## 3. Países del mismo continente que Argentina o Australia
```sql
SELECT name, continent  
FROM world  
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))  
ORDER BY name;
```
## 4. Países con población entre Reino Unido y Alemania
```sql
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```
## 5. Población de cada país europeo como porcentaje de la población de Alemania
```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```
## 6. País con mayor PIB fuera de Europa
```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```
## 7. País más grande por área en cada continente
```sql
SELECT continent, name, area  
FROM world w1  
WHERE area = (SELECT MAX(area) FROM world w2 WHERE w1.continent = w2.continent);
```
## 8. Primer país alfabéticamente en cada continente
```sql
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent);
```
## 9. Continentes donde todos los países tienen menos de 25 millones de habitantes
```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000);
```
## 10. Países cuya población es más de 3 veces la del segundo país más poblado del continente
```sql
SELECT name, continent
FROM world w1
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world w2
    WHERE w1.continent = w2.continent
    AND w2.name != w1.name
);
```
