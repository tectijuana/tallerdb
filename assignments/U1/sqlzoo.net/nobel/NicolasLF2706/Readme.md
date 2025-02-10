# López Félix Nicolás

```Sql

# 1
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950

# 2
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'

# 3
SELECT yr, subject FROM nobel
WHERE winner = 'Albert Einstein'

# 4
SELECT winner FROM nobel
WHERE subject = 'peace'
AND yr >= 2000

# 5
SELECT * FROM nobel
WHERE subject = 'literature'
AND yr >= 1980
AND yr < 1990

# 6
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson',
 'Jimmy Carter', 'Barack Obama')

# 7
SELECT winner FROM nobel
WHERE winner LIKE 'John %'

# 8
SELECT * FROM nobel
WHERE subject = 'physics'
AND yr = '1980'
OR subject = 'chemistry'
AND yr = 1984

# 9
SELECT * FROM nobel
WHERE yr = 1980
AND subject != 'chemistry'
AND subject != 'medicine'

# 10
SELECT * FROM nobel
WHERE subject = 'Medicine'
AND yr < 1910
UNION
SELECT * FROM nobel
WHERE subject = 'Literature'
AND yr >= 2004

# 11
SELECT * FROM nobel
WHERE winner = 'Peter Grünberg'

# 12
SELECT * FROM nobel
WHERE winner = 'Eugene O''Neill'

# 13
SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir %'
ORDER BY -yr, winner

# 14
SELECT winner, subject FROM nobel
WHERE yr = 1984
ORDER BY 
    CASE 
        WHEN subject IN ('Chemistry', 'Physics') THEN 1
        ELSE 0
    END, subject, winner
