*Cruz Hernandez Edgar Eduardo*
# 1. Más grande que Rusia
```sql
SELECT name 
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Russia');
```

# 2. Más ricos que el Reino Unido
```sql
SELECT name  
FROM world  
WHERE continent = 'Europe'  
AND gdp / population > (SELECT gdp / population FROM world WHERE name = 'United Kingdom');
```

# 3. Vecinos de Argentina y Australia
```sql
SELECT name, continent  
FROM world  
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))  
ORDER BY name;
```

# 4. Entre Canadá y Polonia
```sql
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```

# 5. Porcentajes de la población de Alemania
```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```

# 6. Más grande que cualquier país de Europa
```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```

# 7. El más grande de cada continente
```sql
SELECT continent, name, area  
FROM world w1  
WHERE area = (SELECT MAX(area) FROM world w2 WHERE w1.continent = w2.continent);
```

# 8. Primer país de cada continente (alfabéticamente)
```sql
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent)
```

# 9. Continentes donde todos los países tienen una población menor o igual a 25,000,000
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

# 10. Tres veces más grande
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
