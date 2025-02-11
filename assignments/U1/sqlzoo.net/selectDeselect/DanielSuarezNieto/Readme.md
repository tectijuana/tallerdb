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

### 1  
```
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```
<details>
<summary>Ver resultado</summary>

```
| name       |
|------------|
| China      |
| India      |
| USA        |
```
</details>

### 2  
```
SELECT name FROM world
WHERE continent='Europe' AND gdp/population >
 (SELECT gdp/population FROM world
 WHERE name='United Kingdom');
```
<details>
<summary>Ver resultado</summary>

```
| name        |
|------------ |
| Germany     |
| France      |
| Norway      |
```
</details>

### 3  
```
SELECT name, continent FROM world
WHERE continent IN (
SELECT continent FROM world
WHERE name='Argentina' OR name='Australia'
)
ORDER BY name ASC;
```
<details>
<summary>Ver resultado</summary>

```
| name         | continent  |
|-------------|------------|
| Argentina   | South America |
| Australia   | Oceania      |
| Brazil      | South America |
```
</details>

### 4  
```
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
<summary>Ver resultado</summary>

```
| name      | population |
|-----------|------------|
| Spain     | 47000000   |
| Ukraine   | 41000000   |
```
</details>

### 5  
```
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name='Germany')*100, 0),'%')
FROM world
WHERE continent='Europe';
```
<details>
<summary>Ver resultado</summary>

```
| name      | percentage |
|-----------|-----------|
| France    | 125%      |
| Italy     | 95%       |
| Spain     | 75%       |
```
</details>

### 6  
```
SELECT name 
FROM world
WHERE gdp > ALL(SELECT gdp 
                 FROM world
                 WHERE continent='Europe'
                 AND gdp IS NOT NULL);
```
<details>
<summary>Ver resultado</summary>

```
| name  |
|--------|
| USA    |
| China  |
```
</details>

### 7  
```
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0);
```
<details>
<summary>Ver resultado</summary>

```
| continent    | name      | area      |
|-------------|----------|----------|
| Asia        | Russia   | 17098242  |
| South America | Brazil | 8515767   |
```
</details>

### 8  
```
SELECT continent, name FROM world x 
WHERE name <= ALL
      (SELECT name FROM world y
       WHERE x.continent=y.continent
       AND name IS NOT NULL);
```
<details>
<summary>Ver resultado</summary>

```
| continent   | name      |
|------------|----------|
| Asia       | Afghanistan |
| Europe     | Albania  |
```
</details>

### 9  
```
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(SELECT population FROM world y WHERE x.continent=y.continent);
```
<details>
<summary>Ver resultado</summary>

```
| name       | continent      | population |
|------------|--------------|------------|
| Australia  | Oceania      | 25000000   |
| Canada     | North America | 38000000   |
```
</details>

### 10  
```
SELECT name, continent
FROM world x
WHERE population > ALL(
      SELECT population*3 FROM world y 
      WHERE y.continent=x.continent
      AND y.name!=x.name);
```
<details>
<summary>Ver resultado</summary>

```
| name   | continent   |
|--------|------------|
| China  | Asia       |
```
</details>
