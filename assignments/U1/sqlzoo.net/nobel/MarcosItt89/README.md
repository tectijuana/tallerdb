# 🏆 Realizado por Ramirez Guerra 23210648

### 🎖️ Problemas SQL de premios Nobel

En esta sección veremos algunos problemas y sus soluciones, teniendo como base de datos ganadores de premios Nobel. Sus tablas se conforman por:
- 📅 Año en que se ganó (`yr`)
- 📚 Materia en que se ganó (`subject`)
- 🏅 Nombre del ganador (`winner`)

---

## 1️⃣. Mostrar los premios Nobel de 1950 🏆
Este problema pedía corregir la sintaxis que estaba escrita, ya que la que tenía pedía los ganadores del premio Nobel del año 1960. Solo había que cambiar el número.

**Antes:**
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1960
```

**Ahora:**
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```

---

## 2️⃣. Mostrar quién ganó el premio de literatura en 1962 📖
El problema pedía el ganador del premio de literatura de 1962, aunque en el código estaba mal escrito y pedía el ganador del premio de química. Solo había que corregir la sintaxis.

**Antes:**
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'chemistry'
```

**Ahora:**
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
```

---

## 3️⃣. Mostrar el año y la materia en la que ganó Albert Einstein 🧠
De este problema en adelante ya no te daba una codigo a corregir ya es tu propio ingenio 
y era simple al inicio 

```sql
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
```

---

## 4️⃣. Mostrar los ganadores del premio Nobel de la paz desde el año 2000 ☮️
Usando el > que es como decir mayor que y el = para decir mayores e igual el 2000 
```sql
SELECT winner
FROM nobel
WHERE yr >= 2000 
AND subject = 'peace'
```

---

## 5️⃣. Mostrar los premios Nobel de literatura entre 1980 y 1989 📖
Al usar esas 2 condiciones puede elegir pude capturar los datos solicitados
```sql
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'literature'
AND yr >= 1980 
AND yr <= 1989
```

---

## 6️⃣. Mostrar todos los premios Nobel de la paz ganados por ciertos presidentes de EE.UU. 🇺🇸
Con una lista de nombres que me dieron nada mas tenia que hacer la lista y agregarlos 
tuve algunos problemas por confuncion de dedos pero de alli en fuera no hubo mas problemas 

```sql
SELECT *
FROM nobel
WHERE subject = 'Peace'
AND winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson', 'Jimmy Carter',
'Barack Obama');
```

---

## 7️⃣. Mostrar todos los ganadores cuyo nombre comienza con 'John' 🔍
Usando la palabra LIKE 'Palabra a buscar%' y recordando agregar el % del lado que necesites ya sea que este al inicio el nombre o al final
```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John%'
```

---

## 8️⃣. Mostrar premios Nobel de física en 1980 o de química en 1984 ⚛️
Aplicar 2 condiciones a la vez con OR
```sql
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'physics' AND yr = 1980)
OR (subject = 'chemistry' AND yr = 1984);
```

---

## 9️⃣. Mostrar premios Nobel de 1980 que no sean de química ni medicina 🚫

!= O mejor dicho diferente de para excluirlos 
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980
AND subject != 'chemistry'
AND subject != 'medicine'
```

---

## 🔟. Mostrar premios Nobel de medicina antes de 1910 o de literatura desde 2004 📜
Aqui de nuevo era usar dos condiciones y los < >= 
```sql
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'medicine' AND yr < 1910)
   OR (subject = 'literature' AND yr >= 2004);
```

---

## 🔍 11. Mostrar todos los datos de Peter Grünberg 🏅
Simplemente Nombrar correctamente a la persona
```sql
SELECT *
FROM nobel
WHERE winner = 'PETER GRÜNBERG'
```

---

## 🔍 12. Mostrar todos los datos de Eugene O'Neill 🎭
Usando * seleccionas todos los datos de la base de datos y nada mas agregas la condicion que es en este caso de tal persona
```sql
SELECT *
FROM nobel
WHERE winner = 'Eugene O''Neill'
```

---

## 🏅 13. Mostrar los ganadores cuyo nombre comienza con 'Sir' ordenados por año descendente 📜
Fue algo mas complicado pero al implementar el LIKE  con el order By fue sencillo lo demas
```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner;
```

---

## 🏆 14. Mostrar los ganadores del Nobel en 1984 ordenados por prioridad de materia 🎖️
Este sin duda fue un reto pero aplicando todo lo aprendido me ayudo 
```sql
SELECT winner, subject
FROM nobel
WHERE yr = 1984
ORDER BY 
  CASE
    WHEN subject IN ('Physics', 'Chemistry') THEN 1
    ELSE 0
  END,
  subject,
  winner;
```

---
## Bueno eso es todo espero te halla funcionado este documento :3
