*Martinez Guzman Leonardo Rafael 22210318*

## 1. Países con una Población Mayor que Rusia
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```

## 2. Países Europeos con un PIB per Cápita Mayor que el Reino Unido
```sql
SELECT name  
FROM world  
WHERE continent = 'Europe'  
AND gdp / population > (SELECT gdp / population FROM world WHERE name = 'United Kingdom');
```

## 3. Países en los Mismos Continentes que Argentina o Australia
```sql
SELECT name, continent  
FROM world  
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))  
ORDER BY name;
```

## 4. Países con Población Entre la del Reino Unido y Alemania
```sql
SELECT name, population  
FROM world  
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')  
AND population < (SELECT population FROM world WHERE name = 'Germany');
```

## 5. Población de Países Europeos como Porcentaje de la Población de Alemania
```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```

## 6. Países con un PIB Mayor que Cualquier País Europeo
```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```

## 7. País Más Grande por Área en Cada Continente
```sql
SELECT continent, name, area  
FROM world w1  
WHERE area = (SELECT MAX(area) FROM world w2 WHERE w1.continent = w2.continent);
```

## 8. Primer País Alfabéticamente en Cada Continente
```sql
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent);
```

## 9. Continentes Donde Todos los Países Tienen una Población ≤ 25 Millones
```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000);
```

## 10. Países con Más del Triple de Población que Cualquier Otro País en su Continente
```sql
SELECT name, continent
FROM world w1
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world w2
    WHERE w1.continent = w2.continent
    AND w2.name != w1.name
);
