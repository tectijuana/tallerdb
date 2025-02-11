# JUAREZ CASTELAN JOSE

```sql
Problema 1:
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```

```sql
Problema 2:
SELECT winner 
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
```

```sql
Problema 3:
SELECT yr, subject
FROM nobel
 WHERE winner = 'Albert Einstein'
```

```sql
Problema 4:
Select winner winner_peace
FROM nobel
WHERE subject = 'peace'
AND yr >= 2000 

```

```sql
Problema 5:
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'literature' 
AND yr BETWEEN 1980 AND 1989
```

```sql
Problema 6:
SELECT * FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
```

```sql
Problema 7:
SELECT winner FROM nobel
WHERE winner LIKE 'John%' 

```

```sql
Problema 8:
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'literature' 
AND yr BETWEEN 1980 AND 1989
```


```sql
Problema 9:
SELECT * FROM nobel
WHERE (yr = 1980)
AND subject NOT in ('medicine', 'chemistry')
```

```sql
Problema 10:
SELECT * FROM nobel
WHERE (yr<1910 AND subject = 'Medicine')
 OR ( yr>= 2004 AND subject  = 'Litrature');
```

```sql
Problema 11:
SELECT * FROM nobel 
WHERE winner = 'PETER GRÜNBERG'
```

```sql
Problema 12:
SELECT * FROM nobel 
WHERE winner = "Eugene O'Neill"
```

```sql
Problema 13:
SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC , winner ASC;
```

```sql
Problema 14:
SELECT Winner, Subject
FROM nobel
WHERE yr = 1984
ORDER BY CASE WHEN Subject IN ('Chemistry', 'Physics') THEN 1 ELSE 0 END, Subject, Winner;
```
