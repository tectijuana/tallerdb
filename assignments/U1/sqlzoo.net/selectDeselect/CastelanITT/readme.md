# JUAREZ CASTELAN JOSE

## 1. Más grande que Rusia
```sql
Problema 1:
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

## 2. Más rico que el Reino Unido
```sql
Problema 2:
SELECT name
FROM world
WHERE continent ='Europe' AND gdp/population>(
    SELECT gdp/population
    FROM world
    WHERE name ='United Kingdom'
)
```

## 3. Vecinos de Argentina y Australia
```sql
Problema 3:
SELECT name, continent
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    WHERE name IN ('Argentina', 'Australia')
)
ORDER BY name;
```

## 4. Entre Canadá y Polonia
```sql
Problema 4:
SELECT name, population
FROM world
WHERE population > (
    SELECT population
    FROM world
    WHERE name = 'United Kingdom'
)
AND population < (
    SELECT population
    FROM world
    WHERE name = 'Germany'
)
```

## 5. Porcentajes de Alemania
```sql
Problema 5:
SELECT name, CONCAT(ROUND((population/(SELECT population FROM world WHERE name = 'Germany')) * 100), '%') AS percentage
FROM world WHERE continent = 'Europe';
```

## 6. Más grande que todos los países de Europa
```sql
Problema 6:
SELECT name
FROM world
WHERE gdp>ALL( SELECT gdp
FROM world
WHERE continent ='Europe' AND gdp IS NOT NULL
)
```

## 7. Más grande de cada continente
```sql
Problema 7:
SELECT continent, name, area
FROM world w1
WHERE area = (
    SELECT MAX(area)
    FROM world w2
    WHERE w1.continent = w2.continent
)
```

## 8. Primer país de cada continente (en orden alfabético)
```sql
Problema 8:
SELECT continent, name
FROM world w1
WHERE name = (
    SELECT MIN(name)
    FROM world w2
    WHERE w1.continent = w2.continent
)
```

## 9. Preguntas difíciles que utilizan técnicas no cubiertas en secciones anteriores
```sql
Problema 9:
SELECT name, continent, population
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    GROUP BY continent
    HAVING MAX(population) <= 25000000
)
```

## 10. Tres veces más grande
```sql
Problema 10:
SELECT w1.name, w1.continent
FROM world w1
WHERE w1.population > 3 * (
    SELECT MAX(w2.population)
    FROM world w2
    WHERE w2.continent = w1.continent
      AND w2.name != w1.name
)
```
