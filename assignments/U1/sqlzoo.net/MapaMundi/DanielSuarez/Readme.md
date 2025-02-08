# SELECT from WORLD Tutorial

## Nombre: LUIS DANIEL SUAREZ NIETO - 22210356

## Descripción general del ejercicio  

Este conjunto de ejercicios en SQL se centra en la consulta y manipulación de datos de la base de datos `world`, la cual contiene información sobre países, incluyendo su población, PIB, área geográfica y continente. A lo largo de los ejercicios, se utilizan diversas cláusulas y funciones de SQL para filtrar, transformar y analizar los datos de manera efectiva.  

El objetivo principal es reforzar el conocimiento de SQL mediante la práctica de consultas con diferentes criterios y operadores. Algunas de las técnicas clave utilizadas incluyen:  

- **Filtrado de datos con `WHERE`**: Se aplican condiciones sobre atributos como población, PIB, área y nombres de países.  
- **Uso de operadores lógicos (`AND`, `OR`, `XOR`)**: Para combinar múltiples condiciones en las consultas.  
- **Manipulación de cadenas con `LIKE`**: Para buscar coincidencias parciales en los nombres de países y capitales.  
- **Cálculos con columnas numéricas**: Se realizan operaciones matemáticas, como dividir el PIB entre la población para obtener el PIB per cápita.  
- **Redondeo de valores con `ROUND`**: Para mejorar la presentación de los resultados financieros y demográficos.  
- **Uso de funciones de cadena (`LENGTH`, `LEFT`)**: Para analizar y comparar la longitud de los nombres de países y capitales.  
- **Uso de `IN` para simplificar condiciones múltiples**: Para filtrar países específicos sin necesidad de múltiples condiciones `OR`.  

Estos ejercicios no solo ayudan a mejorar la comprensión de SQL, sino que también ofrecen una visión práctica de cómo se pueden analizar grandes volúmenes de datos en bases de datos relacionales.  

Cada problema presenta un escenario distinto que permite explorar la flexibilidad y el poder de SQL en la gestión de datos, lo que es esencial para cualquier analista, desarrollador o científico de datos.  


## Problema 1:
```sql
SELECT name, continent, population FROM world

```
## Problema 2:
```sql
SELECT name FROM world
WHERE population = 64105700

```

## Problema 3:
```sql
SELECT name, gdp/population FROM world
WHERE population >= 200000000;
```
## Problema 4:
```sql
SELECT name, population/1000000 FROM world
WHERE continent = 'South America';
```
## Problema 5:
```sql
Select name, population FROM world
where name in ('France', 'Germany', 'Italy');
```
## Problema 6:
```sql
SELECT name FROM world
WHERE name LIKE '%United%';
```
## Problema 7:
```sql
SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000;
```
## Problema 8:
```sql
Select name, population, area FROM world 
Where area > 3000000 XOR population > 250000000
```
## Problema 9:
```sql
SELECT name, ROUND((population/1000000), 2), ROUND((GDP/1000000000), 2) FROM world 
WHERE continent = 'South America'
```
## Problema 10:
```sql
SELECT name, ROUND((gdp/population), -3) FROM world
WHERE gdp >= 1000000000000;
```
## Problema 11:
```sql
SELECT name, capital FROM world
WHERE LENGTH(name) = LENGTH(capital)

```
## Problema 12:
```sql
SELECT name, capital FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1) AND
name <> capital;

```
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


