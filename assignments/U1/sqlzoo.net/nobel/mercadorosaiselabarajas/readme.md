# Realizado por Rosa Barajas

## 🏆Premios Nobel
Este tutorial trata sobre una tabla de ganadores del premio Nobel:
- 📅 Año en que se ganó (`yr`)
- 📖 Materia en que se ganó (`subject`)
- 🏅 Nombre del ganador (`winner`)

### 1. Ganadores de 1950
👉Cambie la consulta que se muestra para que muestre los premios Nobel de 1950.
```sql
SELECT yr, subject, winner FROM nobel
 WHERE yr = 1950
```

### 2. Literatura de 1962
👉Muestra quién ganó el premio de literatura en 1962.
```sql
SELECT winner FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
```

### 3. Albert Einstein
👉Muestra el año y la materia que le valió a 'Albert Einstein' su premio.
```sql
SELECT yr, subject FROM nobel
WHERE winner = 'Albert Einstein
```

### 4. Premios de la Paz recientes
👉Dar el nombre de los ganadores de la "paz" desde el año 2000, incluido el 2000.
```sql
SELECT winner FROM nobel 
WHERE yr >= 2000
AND subject = 'peace'
```

### 5. La literatura en los años 80
👉Mostrar todos los detalles ( año , materia , ganador ) de los ganadores del premio de literatura de 1980 a 1989 inclusive.
```sql
SELECT winner FROM nobel 
WHERE yr >= 2000
AND subject = 'peace'
```

### 6. Solo presidentes
👉Mostrar todos los detalles de los ganadores presidenciales:
- Teodoro Roosevelt
- Thomas Woodrow Wilson
- Jimmy Carter
- Barack Obama
```sql
SELECT * FROM nobel
WHERE  winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson', 'Jimmy Carter',
'Barack Obama');
```

### 7. John
👉Mostrar los ganadores con el nombre John
```sql
SELECT winner FROM nobel
WHERE winner LIKE 'John%'
```

### 8. Química y Física de diferentes años
👉Muestra el año, la materia y el nombre de los ganadores de física de 1980 junto con los ganadores de química de 1984.
```sql
SELECT yr, subject, winner FROM nobel
WHERE (subject = 'physics' AND yr = 1980)
OR (subject = 'chemistry' AND yr = 1984);
```

### 9. Excluir químicos y médicos
👉Mostrar el año, la materia y el nombre de los ganadores de 1980, excluyendo química y medicina.
```sql
SELECT yr, subject, winner FROM nobel
WHERE yr = 1980
AND subject != 'chemistry'
AND subject != 'medicine'
```

### 10. Medicina temprana, literatura tardía
👉Mostrar año, tema y nombre de las personas que ganaron un premio de "Medicina" en un año temprano (antes de 1910, sin incluir 1910) junto con los ganadores de un premio de "Literatura" en un año posterior (después de 2004, incluido 2004)
```sql
SELECT yr, subject, winner FROM nobel
WHERE (subject = 'medicine' AND yr < 1910)
   OR (subject = 'literature' AND yr >= 2004);
```

### 11. Peter GrÜnberg
👉Encuentre todos los detalles del premio ganado por PETER GRÜNBERG}
```sql
SELECT * FROM nobel
WHERE winner = 'PETER GRÜNBERG'
```

### 12. Eugene O'Neill
👉Encuentre todos los detalles del premio ganado por EUGENE O'NEILL
```sql
SELECT * FROM nobel
WHERE winner = 'Eugene O''Neill'
```

### 13. Ganadores cuyo nombre empiece con "Sir"
👉Enumere los ganadores, el año y la materia donde el ganador comience con Sir . Muestre el más reciente primero y luego por orden de nombre.
```sql
SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner;
```

### 14. Ganadores de 1984
👉Mostrar los ganadores de 1984 y el tema ordenados por tema y nombre del ganador; pero enumerar química y física al final.
```sql
SELECT winner, subject FROM nobel
WHERE yr = 1984
ORDER BY 
  CASE
    WHEN subject IN ('Physics', 'Chemistry') THEN 1
    ELSE 0
  END,
  subject,
  winner;
  ```
