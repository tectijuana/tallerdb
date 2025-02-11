### Elaborado por : Meza Rodriguez Eduardo Manuel #21211997

## Problema 1:

```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```
### Explicacion:
- Esta consulta selecciona el año, la categoría y el ganador del premio Nobel correspondiente al año 1950.
## Problema 2:
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
```
### Explicacion:
- Esta consulta obtiene el nombre del ganador del Premio Nobel de Literatura en el año 1962.

## Problema 3:
```sql
select yr, subject
from nobel
where winner = 'Albert Einstein'
```
### Explicacion:
- Se busca en qué año y en qué categoría Albert Einstein ganó el Premio Nobel.

## Problema 4:
```sql
select winner from nobel
where yr >= 2000
and subject = 'peace'
```
### Explicacion:
- Esta consulta lista los ganadores del Premio Nobel de la Paz desde el año 2000 en adelante.
## Problema 5:
```sql
select * from nobel
where subject = 'literature'
and (yr >= 1980 and yr <=1989)
```
### Explicacion:
- Devuelve todos los datos de los premios Nobel de Literatura otorgados entre los años 1980 y 1989.
## Problema 6:
```sql
SELECT * FROM nobel
where winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson','Jimmy Carter','Barack Obama')
```
- Se recupera la información de los premios Nobel otorgados a los expresidentes de EE.UU. mencionados en la consulta.
## Problema 7:
```sql
select winner from nobel
where winner like 'John%'
```
### Explicacion:
-Selecciona todos los ganadores cuyo nombre comienza con "John".
## Problema 8:
```sql
select * from nobel
where (subject = 'physics' and yr = 1980)
or (subject = 'chemistry' and yr= 1984)
```
### Explicacion:
- Se recupera la información de los premios Nobel en la categoría Física en 1980 y Química en 1984.
## Problema 9:
```sql
select * from nobel
where yr = 1980
and subject not IN ('chemistry', 'medicine')
```
### Explicacion:
- Obtiene todos los premios Nobel otorgados en 1980, excepto los de Química y Medicina.
## Problema 10 :
```sql
select * from nobel
where (subject = 'Medicine' and yr < 1910)
or (subject = 'literature' and yr >= 2004)
```
### Explicacion:
- Selecciona todos los premios Nobel de Medicina antes de 1910 y los de Literatura desde 2004 en adelante.
## Problema 11:
```sql
select * from nobel
where winner = 'PETER GRÜNBERG'
```
### Explicacion:
- Devuelve toda la información relacionada con Peter Grünberg, ganador de un premio Nobel.
## Problema 12:
```sql
select * from nobel
where winner = 'EUGENE O''NEILL'
```
### Explicacion:
- Consulta los datos del premio Nobel otorgado a Eugene O’Neill. Se usa doble apóstrofe ('') para manejar la comilla simple en el nombre.
## Problema 13:
```sql
select winner, yr, subject from nobel
where winner like 'Sir%'
order by yr DESC, winner ASC
```
### Explicacion:
- Lista los ganadores con el título "Sir" en su nombre, ordenando por año en orden descendente y luego por nombre en orden ascendente.
## Problema 14:
```sql
SELECT winner, subject FROM nobel
 WHERE yr=1984
 ORDER BY subject in ('chemistry','physics'), subject, winner
```
### Explicacion:
- Ordena los premios Nobel del año 1984, dando prioridad a Química y Física, luego ordenando por categoría y finalmente por nombre del ganador.
