### Realizado por : Brian Guadalupe Tellez Escobar C21211400 07/02/2025

Este documento describe una serie de consultas SQL sobre la tabla `nobel`, que contiene información sobre los premios Nobel, incluyendo el año (`yr`), la categoría (`subject`) y el ganador (`winner`).


## Problema 1 

```
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```
Esta consulta selecciona el año, la categoría y el ganador del premio Nobel correspondiente al año 1950.
## Problema 2
```
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
```
Esta consulta obtiene el nombre del ganador del Premio Nobel de Literatura en el año 1962.
## Problema 3
```
select yr, subject
from nobel
where winner = 'Albert Einstein'
```
Se busca en qué año y en qué categoría Albert Einstein ganó el Premio Nobel.
## Problema 4
```
select winner from nobel
where yr >= 2000
and subject = 'peace'
```
Esta consulta lista los ganadores del Premio Nobel de la Paz desde el año 2000 en adelante.
## Problema 5
```
select * from nobel
where subject = 'literature'
and (yr >= 1980 and yr <=1989)
```
Devuelve todos los datos de los premios Nobel de Literatura otorgados entre los años 1980 y 1989.
## Problema 6
```
SELECT * FROM nobel
where winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson','Jimmy Carter','Barack Obama')
```
Se recupera la información de los premios Nobel otorgados a los expresidentes de EE.UU. mencionados en la consulta.
## Problema 7
```
select winner from nobel
where winner like 'John%'
```
Selecciona todos los ganadores cuyo nombre comienza con "John".
## Problema 8
```
select * from nobel
where (subject = 'physics' and yr = 1980)
or (subject = 'chemistry' and yr= 1984)
```
Se recupera la información de los premios Nobel en la categoría Física en 1980 y Química en 1984.
## Problema 9
```
select * from nobel
where yr = 1980
and subject not IN ('chemistry', 'medicine')
```
Obtiene todos los premios Nobel otorgados en 1980, excepto los de Química y Medicina.
## Problema 10 
```
select * from nobel
where (subject = 'Medicine' and yr < 1910)
or (subject = 'literature' and yr >= 2004)
```
Selecciona todos los premios Nobel de Medicina antes de 1910 y los de Literatura desde 2004 en adelante.
## Problema 11
```
select * from nobel
where winner = 'PETER GRÜNBERG'
```
Devuelve toda la información relacionada con Peter Grünberg, ganador de un premio Nobel.
## Problema 12 
```
select * from nobel
where winner = 'EUGENE O''NEILL'
```
Consulta los datos del premio Nobel otorgado a Eugene O’Neill. Se usa doble apóstrofe ('') para manejar la comilla simple en el nombre.
## Problema 13
```
select winner, yr, subject from nobel
where winner like 'Sir%'
order by yr DESC, winner ASC
```
Lista los ganadores con el título "Sir" en su nombre, ordenando por año en orden descendente y luego por nombre en orden ascendente.
## Problema 14
```
SELECT winner, subject FROM nobel
 WHERE yr=1984
 ORDER BY subject in ('chemistry','physics'), subject, winner
```
Ordena los premios Nobel del año 1984, dando prioridad a Química y Física, luego ordenando por categoría y finalmente por nombre del ganador.

## 📢 Conclusión
Este conjunto de consultas SQL demuestra diferentes técnicas para recuperar información específica de una base de datos de premios Nobel. Se utilizan filtros (WHERE), combinaciones de condiciones (AND, OR, NOT IN), patrones de búsqueda (LIKE), ordenación (ORDER BY) y operadores de comparación (IN, BETWEEN).
