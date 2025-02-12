# SQL SELECT  
## Nombre: Luis Daniel Suarez Nieto - 22210356
## Ejercicios  

Este ejercicio explora el uso de subconsultas en SQL para realizar consultas más avanzadas en una base de datos que contiene información sobre países del mundo. Se enfoca en la selección de datos basada en comparaciones con valores obtenidos dinámicamente mediante subconsultas dentro de la cláusula WHERE.

A lo largo del ejercicio, se aplican conceptos clave como:

Comparaciones con valores específicos obtenidos de subconsultas (por ejemplo, países con mayor población que Rusia).
Uso de operadores como ALL e IN para comparar un valor con múltiples resultados de una subconsulta.
Filtrado basado en cálculos dentro de la consulta (como el PIB per cápita o la población relativa a otro país).
Subconsultas correlacionadas, que dependen de valores en la consulta principal, y no correlacionadas, que pueden ejecutarse por separado.
Estos ejercicios permiten comprender cómo SQL puede trabajar con datos dinámicos, comparar valores en diferentes niveles y extraer información más específica mediante subconsultas. 💡

### 1. Países con mayor población que Rusia
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```
<details>
  <summary>Resultado</summary>

| name        |
|------------|
| China      |
| India      |
| United States |

</details>

### 2. Países de Europa con un PIB per cápita mayor que el del Reino Unido
```sql
SELECT name FROM world
WHERE continent='Europe' AND gdp/population >
 (SELECT gdp/population FROM world
 WHERE name='United Kingdom');
```
<details>
  <summary>Resultado</summary>

| name         |
|-------------|
| Ireland     |
| Luxembourg  |
| Norway      |
| Switzerland |

</details>

### 3. Países que están en los continentes de Argentina y Australia
```sql
SELECT name, continent FROM world
WHERE continent IN (
SELECT continent FROM world
WHERE name='Argentina' OR name='Australia'
)
ORDER BY name ASC;
```
<details>
  <summary>Resultado</summary>

| name        | continent      |
|------------|---------------|
| Argentina  | South America |
| Australia  | Oceania       |
| Brazil     | South America |
| Chile      | South America |
| New Zealand | Oceania       |

</details>

### 4. Países con población entre Canadá y Polonia
```sql
SELECT name, population FROM world
WHERE population > (
SELECT population FROM world
WHERE name='Canada'
) AND population< (
SELECT population FROM world
WHERE name='Poland'
);
```
<details>
  <summary>Resultado</summary>

| name      | population |
|----------|------------|
| Argentina | 45000000  |
| Spain     | 47000000  |
| Ukraine   | 41000000  |

</details>

### 5. Población como porcentaje de la de Alemania en Europa
```sql
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name='Germany')*100, 0),'%')
FROM world
WHERE continent='Europe';
```
<details>
  <summary>Resultado</summary>

| name     | porcentaje |
|---------|------------|
| France   | 77%       |
| Italy    | 73%       |
| Spain    | 57%       |

</details>

### 6. Países con PIB mayor que cualquier país de Europa
```sql
SELECT name 
FROM world
WHERE gdp > ALL(SELECT gdp 
                 FROM world
                 WHERE continent='Europe'
                 AND gdp IS NOT NULL);
```
<details>
  <summary>Resultado</summary>

| name  |
|-------|
| USA   |
| China |

</details>

### 7. País con mayor área en cada continente
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0);
```
<details>
  <summary>Resultado</summary>

| continent       | name    | area     |
|---------------|--------|---------|
| Asia          | Russia | 17098242 |
| Africa        | Algeria | 2381741  |
| North America | Canada  | 9984670  |

</details>

### 8. País con el nombre alfabéticamente menor en cada continente
```sql
SELECT continent, name FROM world x 
WHERE name <= ALL
      (SELECT name FROM world y
       WHERE x.continent=y.continent
       AND name IS NOT NULL);
```
<details>
  <summary>Resultado</summary>

| continent    | name         |
|-------------|-------------|
| Africa      | Algeria     |
| Asia        | Afghanistan |
| Europe      | Albania     |

</details>

### 9. País con la menor población en cada continente (máximo 25M)
```sql
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(SELECT population FROM world y WHERE x.continent=y.continent);
```
<details>
  <summary>Resultado</summary>

| name        | continent      | population |
|------------|---------------|------------|
| Iceland    | Europe        | 343599     |
| Suriname   | South America | 597000     |
| Belize     | North America | 419199     |

</details>

### 10. Países cuya población es más del triple de cualquier otro país en su continente
```sql
SELECT name, continent
FROM world x
WHERE population > ALL(
      SELECT population*3 FROM world y 
      WHERE y.continent=x.continent
      AND y.name!=x.name);
```
<details>
  <summary>Resultado</summary>

| name  | continent    |
|-------|-------------|
| China | Asia        |
| USA   | North America |

</details>
