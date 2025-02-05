# Devuelve todas las filas de la tabla derecha con coincidencias opcionales.
## Devuelve todas las filas de la tabla derecha con coincidencias opcionales.

### El RIGHT JOIN en SQL se usa para obtener todos los registros de la tabla derecha y los registros coincidentes de la tabla izquierda. Si no hay coincidencias, las columnas de la tabla izquierda se completan con NULL.

Ejemplo de RIGHT JOIN:

Supongamos que tenemos dos tablas:

Tabla A
--------------------------------
- a
- 3
- 4

Tabla B
--------------------------------
- b
- 3
- 4
- 5
- 6

Si ejecutamos la consulta SQL:

```sql
SELECT * FROM A RIGHT OUTER JOIN B ON A.a = B.b;
El resultado sería:

a	b
3	3
4	4
NULL	5
NULL	6
```
Esto significa que RIGHT JOIN muestra todos los valores de la tabla B y completa con NULL aquellos valores de A que no tienen coincidencias​.
