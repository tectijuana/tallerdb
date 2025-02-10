# Elaborado por Emmanuel Del Angel - 21210366
### Actividad - Practica de SQL con la herramienta de apoyo SQLZoo: Problemas de premios nobel
- Este tutorial se trabaja con una sola tabla (Ganadores del Premio Nobel) usando la instrucción `SELECT`
```sql
nobel(yr, subject, winner)
```
Donde:
- **`yr`** = año
- **`subject`** = asunto
- **`winner`** = ganador

### Problemas del 1 al 14
**Problema 1: Ganadores de 1950**
```sql
# Problema 1: Cambie la consulta que se muestra para que muestre los premios Noble de 1950.
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar el año, asunto y ganador de solo los años de 1950.

**Problema 2: 1962 Literatura**
```sql
# Problema 2: Mostrar quién gano el premio de literatura en 1962.
SELECT winner
FROM nobel
WHERE yr = 1962
AND subject = 'Literature'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo el nombre del ganador que es **"John Steinbeck"**.

**Problema 3: Albert Einstein**
```sql
# Problema 3: Muestre el año y el tema que le valio el premio a "Albert Einstein".
SELECT yr, subject
FROM nobel
WHERE yr = yr
AND winner = 'Albert Einstein'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo el año y asunto.

**Problema 4: Premios de la paz recientes**
```sql
# Problema 4: Menciona el nombre de los ganadores de la "paz" desde el año 2000, incluido el 2000.
SELECT winner
FROM nobel
WHERE yr >= 2000
AND subject = 'peace'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo los ganadores.

**Problema 5: La literatura en la década de 1980**
```sql
# Problema 5: Mostrar todos los detalles (**año,tema,ganador**) de los ganadores del premio de literatura de 1980 a 1989 inclusive.
SELECT yr, subject, winner
FROM nobel
WHERE yr >= 1980
AND yr <= 1989
AND subject = 'Literature'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo el año, asunto y ganador pero solo de 1980 a 1989.

**Problema 6: Solo presidentes**
```sql
# Problema 6: Mostrar todos los detalles de los ganadores presidenciales: Theodore Roosevelt, Thomas Woodrow Wilson, Jimmy Carter, Barack Obama.
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Thomas Woodrow Wilson',
                  'Jimmy Carter',
                  'Barack Obama')
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo muestra el año, asunto y ganador pero solo de los presidentes.

**Problema 7: John**
```sql
# Problema 7: Mostrar los ganadores con el nombre de pila John.
SELECT winner 
FROM nobel
WHERE winner LIKE ('John%')
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo muestra ganadores con nombre de pila John con la herramienta `LIKE`.

**Problema 8: Química y Física de diferentes años**
```sql
# Problema 8: Muestre el año, la materia y el nombre de los ganadores de física de 1980 junto con los ganadores de química de 1984.
SELECT * FROM nobel
WHERE (subject = 'physics' AND yr = 1980)
OR (subject = 'chemistry' AND yr = 1984);
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo muestra el año, asunto y ganador pero usando la herramienta `OR`

**Problema 9: Excluir químicos y médicos**
```sql
# Problema 9: Muestre el año, el tema y el nombre de los ganadores de 1980, excluyendo química y medicina.
SELECT * FROM nobel
WHERE yr = 1980
AND subject != 'chemistry'
AND subject != 'medicine'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo el año, asunto y ganador utilizando el la herramienta `!`

**Problema 10: Medicina Temprana, Literatura Tardía**
```sql
# Problema 10:
SELECT * FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910)
OR (subject = 'Literature' AND yr >= 2004); 
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo año, asunto y ganador con ciertas condiciones.

**Problema 11: Umlaut**
```sql
# Problema 11: Encuentra todos los detalles del premio ganado por PETER GRÜNBERG.
SELECT * FROM nobel
WHERE winner = 'PETER GRÜNBERG'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo los datos de PETER GRÜNBERG

**Problema 12: Apóstrofo**
```sql
# Problema 12: Encuentra todos los detalles del premio ganado por EUGENE O'NEILL
SELECT * FROM nobel
WHERE winner = 'EUGENE O''NEILL'
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo los datos Eugenio O'Neill

**Problema 13: Cabelleros del reino**
```sql
# Problema 13: Caballeros en orden.
SELECT winner, yr, subject 
FROM nobel
WHERE winner LIKE ('Sir%')
ORDER BY yr DESC, winner;
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo el orden de los caballeros que se encuentran en la tabla

**Problema 14: La química y la fisica son las ultimas**
```sql
# Problema 14:
SELECT winner, subject
 FROM nobel
  WHERE yr = 1984
 ORDER BY subject IN ('chemistry','physics'), subject, winner
```
**Detalles:**
- Estas instrucciones nos da como resultado el mostrar solo los ganadores de 1984 y el tema ordenados.
