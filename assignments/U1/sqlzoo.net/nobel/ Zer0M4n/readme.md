## 22211575 - CESAR GONZALEZ SALAZAR

A continuación se presentan todos los problemas y sus consultas SQL.

---

## Problema 1
**Cambiar la consulta mostrada para que muestre los premios Nobel del año 1950.**

```sql
SELECT yr, subject, winner FROM nobel
WHERE yr = 1950;
```

## Problema 2

Show who won the 1962 prize for literature.
```sql
SELECT winner FROM nobel
WHERE yr = 1962 AND subject = 'literature'
```

## Problema 3

Show the year and subject that won 'Albert Einstein' his prize.
```sql
SELECT yr, subject  FROM nobel WHERE winner = 'Albert Einstein'
```

## Problema 4

Give the name of the 'peace' winners since the year 2000, including 2000.
```sql
SELECT winner FROM nobel WHERE yr >= 2000 AND subject = 'peace'
```

## Problema 5

Show all details (yr, subject, winner) of the literature prize winners for 1980 to 1989 inclusive.
```sql
SELECT * FROM nobel WHERE yr >= 1980 AND yr <= 1989 AND subject = 'literature'

```

## Problema 6

Show all details of the presidential winners:

 - Theodore Roosevelt 
 - Thomas Woodrow Wilson 
 - Jimmy Carter 
 - Barack Obama
```sql
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Jimmy Carter',
                  'Barack Obama')

```

## Problema 7

Show the winners with first name John
```sql
SELECT winner FROM nobel WHERE winner LIKE 'John%'
```

## Problema 8

Show the year, subject, and name of physics winners for 1980 together with the chemistry winners for 1984.
```sql
SELECT * FROM nobel
WHERE (subject = 'chemistry' AND yr = 1984) OR (yr = 1980 AND subject = 'physics');
```

## Problema 9

Show the year, subject, and name of winners for 1980 excluding chemistry and medicine
```sql
SELECT * FROM nobel WHERE yr = 1980 AND subject NOT IN ('chemistry','medicine')
```

## Problema 10

Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)

```sql
SELECT * FROM nobel
WHERE (subject  = 'Medicine' AND yr < 1910) OR (subject = 'Literature' AND yr >= 2004)
```
## Problema 11

Find all details of the prize won by PETER GRÜNBERG


```sql
SELECT * FROM nobel
WHERE winner LIKE 'PETER GRÜNBERG'
```

## Problema 12

Find all details of the prize won by EUGENE O'NEILL
```sql
SELECT * FROM nobel
WHERE winner = 'Eugene O''Neill'
```

## Problema 13

Knights in order

List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'sir%' ORDER BY yr DESC, winner
```

## Problema 14

The expression subject IN ('chemistry','physics') can be used as a value - it will be 0 or 1.

Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last.

```sql
SELECT winner, subject FROM nobel
WHERE yr=1984 ORDER BY subject IN ('Physics','Chemistry'),subject,winner
```
