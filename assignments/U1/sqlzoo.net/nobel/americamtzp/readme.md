# America Martinez Perez #21211991

## CONSULTAS SQL

1. **Obtener los premios Nobel de 1950:**  
   ```sql
   SELECT yr, subject, winner
   FROM nobel
   WHERE yr = 1950;
   ```

2. **Obtener el ganador del premio Nobel de Literatura en 1962:**  
   ```sql
   SELECT winner
   FROM nobel
   WHERE yr = 1962
   AND subject = 'Literature';
   ```

3. **Obtener el año y la categoría del premio Nobel ganado por Albert Einstein:**  
   ```sql
   SELECT yr, subject
   FROM nobel
   WHERE winner = 'Albert Einstein';
   ```

4. **Obtener los ganadores del premio Nobel de la Paz desde el año 2000:**  
   ```sql
   SELECT winner
   FROM nobel
   WHERE subject = 'Peace'
   AND yr >= 2000;
   ```

5. **Obtener los premios Nobel de Literatura entre 1980 y 1989:**  
   ```sql
   SELECT yr, subject, winner  
   FROM nobel  
   WHERE subject = 'Literature'  
   AND yr BETWEEN 1980 AND 1989;
   ```

6. **Obtener los ganadores del premio Nobel entre un grupo de personas específicas:**  
   ```sql
   SELECT * FROM nobel
   WHERE winner IN ('Theodore Roosevelt',
                    'Thomas Woodrow Wilson',
                    'Jimmy Carter',
                    'Barack Obama');
   ```

7. **Obtener los ganadores cuyo nombre comienza con 'John':**  
   ```sql
   SELECT winner
   FROM nobel
   WHERE winner LIKE 'John%';
   ```

8. **Obtener los premios Nobel de Física en 1980 y de Química en 1984:**  
   ```sql
   SELECT yr, subject, winner
   FROM nobel
   WHERE (subject = 'Physics' AND yr = 1980)  
      OR (subject = 'Chemistry' AND yr = 1984);
   ```

9. **Obtener los premios Nobel del año 1980 excluyendo Química y Medicina:**  
   ```sql
   SELECT yr, subject, winner
   FROM nobel
   WHERE yr = 1980  
   AND subject NOT IN ('Chemistry', 'Medicine');
   ```

10. **Obtener premios Nobel de Medicina antes de 1910 y de Literatura desde 2004 en adelante:**  
    ```sql
    SELECT yr, subject, winner
    FROM nobel
    WHERE (subject = 'Medicine' AND yr < 1910) OR
    (subject = 'Literature' AND yr >= 2004);
    ```

11. **Obtener el premio Nobel ganado por Peter Grünberg:**  
    ```sql
    SELECT yr, subject, winner
    FROM nobel
    WHERE winner = 'PETER GRÜNBERG';
    ```

12. **Obtener el premio Nobel ganado por Eugene O'Neill:**  
    ```sql
    SELECT yr, subject, winner
    FROM nobel
    WHERE winner = "EUGENE O'NEILL";
    ```

13. **Obtener los ganadores con título 'Sir', ordenados por año descendente y nombre ascendente:**  
    ```sql
    SELECT winner, yr, subject  
    FROM nobel  
    WHERE winner LIKE 'Sir%'  
    ORDER BY yr DESC, winner ASC;
    ```

14. **Ordenar los ganadores del Nobel de 1984, priorizando Química y Física:**  
    ```sql
    SELECT winner, subject  
    FROM nobel
    WHERE yr = 1984  
    ORDER BY
        CASE  
            WHEN subject IN ('Chemistry', 'Physics') THEN 1  
            ELSE 0  
        END,  
        subject ASC,  
        winner ASC;
    ```

