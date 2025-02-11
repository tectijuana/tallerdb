### Elaborado por: Meza Rodriguez Eduardo Manuel #21211997
## Problema 1:

```sql
SELECT name, continent, population FROM world
```
### Explicación:
- Este código selecciona las columnas name, continent y population de la tabla world. Es una consulta básica que devuelve todos los registros de la tabla con estas tres columnas.

## Problema 2:
```sql

SELECT name FROM world
WHERE population = 64105700

```

### Explicación:
- Este código selecciona la columna name de la tabla world donde la población (population) es exactamente igual a 64,105,700. Es una consulta con una condición específica en la cláusula WHERE.

## Problema 3:
```sql

SELECT name, gdp/population FROM world
WHERE population >= 200000000;

```

### Explicación:
- Este código selecciona el nombre (name) y el PIB per cápita (gdp/population) de los países en la tabla world donde la población es mayor o igual a 200,000,000. La columna gdp/population es una operación matemática que calcula el PIB per cápita.

## Problema 4:
```sql

SELECT name, population/1000000 FROM world
WHERE continent = 'South America';

```
### Explicación:
- Este código selecciona el nombre (name) y la población en millones (population/1000000) de los países en la tabla world que pertenecen al continente South America. La población se divide por 1,000,000 para convertirla a millones.

## Problema 5:
```sql

Select name, population FROM world
where name in ('France', 'Germany', 'Italy');

```
### Explicación:
- Este código selecciona el nombre (name) y la población (population) de los países en la tabla world cuyo nombre es France, Germany o Italy. La cláusula IN se utiliza para filtrar por múltiples valores.

## Problema 6:
```sql

SELECT name FROM world
WHERE name LIKE '%United%';

```
### Explicación:
- Este código selecciona el nombre (name) de los países en la tabla world cuyo nombre contiene la palabra United. El operador LIKE con % se utiliza para buscar patrones en cadenas de texto.

## Problema 7:
```sql

SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000;

```
### Explicación:
- Este código selecciona el nombre (name), la población (population) y el área (area) de los países en la tabla world donde el área es mayor a 3,000,000 o la población es mayor a 250,000,000. La cláusula OR permite que se cumpla cualquiera de las dos condiciones.

## Problema 8:
```sql

Select name, population, area FROM world 
Where area > 3000000 XOR population > 250000000

```
### Explicación:
- Este código selecciona el nombre (name), la población (population) y el área (area) de los países en la tabla world donde se cumple una de las dos condiciones, pero no ambas. El operador XOR (OR exclusivo) asegura que solo una de las condiciones sea verdadera.

## Problema 9:
```sql

SELECT name, ROUND((population/1000000), 2), ROUND((GDP/1000000000), 2) FROM world 
WHERE continent = 'South America'

```

### Explicación:
- Este código selecciona el nombre (name), la población en millones redondeada a 2 decimales (ROUND(population/1000000, 2)) y el PIB en billones redondeado a 2 decimales (ROUND(GDP/1000000000, 2)) de los países en la tabla world que pertenecen al continente South America.

## Problema 10:
```sql

SELECT name, ROUND((gdp/population), -3) FROM world
WHERE gdp >= 1000000000000;

```
### Explicación:
- Este código selecciona el nombre (name) y el PIB per cápita redondeado a la unidad de mil (ROUND(gdp/population, -3)) de los países en la tabla world donde el PIB es mayor o igual a 1,000,000,000,000. El redondeo a -3 significa que se redondea a la unidad de mil.

## Problema 11:
```sql

SELECT name, capital FROM world
WHERE LENGTH(name) = LENGTH(capital)
```
### Explicación:
- Este código selecciona el nombre (name) y la capital (capital) de los países en la tabla world donde la longitud del nombre del país es igual a la longitud del nombre de la capital. La función LENGTH se utiliza para obtener la longitud de una cadena de texto.

### Problema 12:
```sql

SELECT name, capital FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1) AND
name <> capital;
```
### Explicación:
- Este código selecciona el nombre (name) y la capital (capital) de los países en la tabla world donde la primera letra del nombre del país es igual a la primera letra de la capital, pero el nombre del país no es igual al nombre de la capital. La función LEFT se utiliza para obtener los primeros caracteres de una cadena.

## Problema 13:
```sql

SELECT name FROM world
WHERE 
name LIKE '%a%' AND
name LIKE '%e%' AND
name LIKE '%i%' AND
name LIKE '%o%' AND
name LIKE '%u%' AND
name NOT LIKE '% %';
```
### Explicación:
- Este código selecciona el nombre (name) de los países en la tabla world donde el nombre contiene todas las vocales (a, e, i, o, u) y no contiene espacios (% %). La cláusula LIKE se utiliza para buscar patrones en cadenas de texto.
