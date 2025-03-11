# PADRON JIMENEZ NICOL JHAMYLETD 

## 📍​📍​/ Nobel/ Nobel Laureates
We continue practicing simple SQL queries on a single table.
This tutorial is concerned with a table of Nobel prize winners:
```
nobel(yr, subject, winner)
```

## Winners from 1950
### 📌​Problema 1: Change the query shown so that it displays Nobel prizes for 1950.
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```
📍​NOTA: Esta consulta selecciona el año, la categoría y el ganador del premio Nobel correspondiente al año 1950.

## 1962 Literature
### 📌​Problema 2: Show who won the 1962 prize for literature.
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
```
📍​NOTA: Esta consulta obtiene el nombre del ganador del Premio Nobel de Literatura en el año 1962.

## Albert Einstein
### 📌​Problema 3: Show the year and subject that won 'Albert Einstein' his prize.
```sql
select yr, subject
from nobel
where winner = 'Albert Einstein'
```
📍​NOTA: Se busca en qué año y en qué categoría Albert Einstein ganó el Premio Nobel.

## Recent Peace Prizes
### 📌​Problema 4: Give the name of the 'peace' winners since the year 2000, including 2000.
```sql
select winner from nobel
where yr >= 2000
and subject = 'peace'
```
📍​NOTA: Esta consulta lista los ganadores del Premio Nobel de la Paz desde el año 2000 en adelante.

## Literature in the 1980's
### 📌​Problema 5: Show all details (yr, subject, winner) of the literature prize winners for 1980 to 1989 inclusive.
```sql
select * from nobel
where subject = 'literature'
and (yr >= 1980 and yr <=1989)
```
📍​NOTA: Devuelve todos los datos de los premios Nobel de Literatura otorgados entre los años 1980 y 1989.

## Only Presidents
### 📌​Problema 6: Show all details of the presidential winners:
Theodore Roosevelt,
Thomas Woodrow Wilson,
Jimmy Carter,
Barack Obama
```sql
SELECT * FROM nobel
where winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson','Jimmy Carter','Barack Obama')
```
📍​NOTA: Se recupera la información de los premios Nobel otorgados a los expresidentes de EE.UU. mencionados en la consulta.

## John
### Problema 7: Show the winners with first name John
```sql
select winner from nobel
where winner like 'John%'
```
📍​NOTA: Selecciona todos los ganadores cuyo nombre comienza con "John".

## Chemistry and Physics from different years
### 📌​Problema 8: Show the year, subject, and name of physics winners for 1980 together with the chemistry winners for 1984.
```sql
select * from nobel
where (subject = 'physics' and yr = 1980)
or (subject = 'chemistry' and yr= 1984)
```
📍​NOTA: Se recupera la información de los premios Nobel en la categoría Física en 1980 y Química en 1984.

## Exclude Chemists and Medics
### Problema 9: Show the year, subject, and name of winners for 1980 excluding chemistry and medicine
```sql
select * from nobel
where yr = 1980
and subject not IN ('chemistry', 'medicine')
```
📍​NOTA: Obtiene todos los premios Nobel otorgados en 1980, excepto los de Química y Medicina.

## Early Medicine, Late Literature
### 📌​Problema 10: Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
select * from nobel
where (subject = 'Medicine' and yr < 1910)
or (subject = 'literature' and yr >= 2004)
```
📍​NOTA: Selecciona todos los premios Nobel de Medicina antes de 1910 y los de Literatura desde 2004 en adelante.

## Umlaut
### 📌​Problema 11: Find all details of the prize won by PETER GRÜNBERG
```sql
select * from nobel
where winner = 'PETER GRÜNBERG'
```
📍​NOTA: Devuelve toda la información relacionada con Peter Grünberg, ganador de un premio Nobel.

## Apostrophe
### 📌​Problema 12: Find all details of the prize won by EUGENE O'NEILL
```sql
select * from nobel
where winner = 'EUGENE O''NEILL'
```
📍​NOTA: Consulta los datos del premio Nobel otorgado a Eugene O’Neill. Se usa doble apóstrofe ('') para manejar la comilla simple en el nombre.

## Knights of the realm
### 📌​Problema 13: List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
select winner, yr, subject from nobel
where winner like 'Sir%'
order by yr DESC, winner ASC
```
📍​NOTA: Lista los ganadores con el título "Sir" en su nombre, ordenando por año en orden descendente y luego por nombre en orden ascendente.

## Chemistry and Physics last
### 📌​Problema 14:  The expression subject IN ('chemistry','physics') can be used as a value - it will be 0 or 1. Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last.
```sql
SELECT winner, subject FROM nobel
 WHERE yr=1984
 ORDER BY subject in ('chemistry','physics'), subject, winner
```
📍​NOTA: Ordena los premios Nobel del año 1984, dando prioridad a Química y Física, luego ordenando por categoría y finalmente por nombre del ganador.
