
# Actividad 1: Elabora una participacion de una recolección de Tips de Bases de datos relacionales, el docente ya tiene 8 entradas elaboradas, le asignamos una participación.

## ¿Donde esta el generador del recurso?
- Ocupa registarse en ChatGtp para invocarlo el GTP https://chatgpt.com/g/g-Ir4EmYe0j-database-mentor

## ¿Cómo contribuir a el repositorio?
- En URL de donde deja su contribucion via GitHub https://github.com/tectijuana/tallerdb/tree/main/resources/tutorials/tips
- Recuerde poner el numero seguido de un guión por la descripción
- En en encabezado dentro del documento en MARKDOWN estará su nombre
- Docente revisa el archivo y pasa la calificación manualmente.
- Docente hace PULL al repositorio para la integración especificamente a ese directorio, fuera de este el trabajo esta invalido.

## EJEMPLO GENERICO DEL PROCEDIMIENTO Fork-Pull de GITHUB
- **Loom 1** - (Estudiante Fork) [https://www.loom.com/share/6835069706494761a6828d4c3b053a21?sid=87c4a764-40b9-4606-8cb9-1235679380ca](https://www.loom.com/share/6835069706494761a6828d4c3b053a21?sid=f0c66887-f17a-4b30-8a58-6d1b2735b432)
- **Loom 2** - (docente Pull) [https://www.loom.com/share/8f26a0e6203d4be68ce65b07c5b5a077?sid=e675e02a-4532-4080-bcf2-5327ddcfc532](https://www.loom.com/share/8f26a0e6203d4be68ce65b07c5b5a077?sid=f9b4f1d5-6757-4949-9647-e3a778ba0f7f)

# LISTA DE ACTIVIDADES


| ID         | Número | Descripción                                      | Explicación |
|------------|--------|--------------------------------------------------|-------------|
| 22211514   | 9      | Usa **EXPLAIN** para analizar consultas          | Verifica cómo se ejecuta tu consulta y optimízala identificando cuellos de botella. |
| 22211519   | 10     | Usa **DISTINCT** para eliminar duplicados        | Devuelve filas únicas de tu consulta sin valores repetidos. |
| C22211889  | 11     | Aprovecha **INNER JOIN** para combinaciones precisas | Conecta tablas con condiciones estrictas entre columnas clave. |
| 20211759   | 12     | Utiliza **LEFT JOIN** para incluir datos faltantes | Muestra todas las filas de la tabla izquierda aunque no tengan coincidencias. |
| 22211544   | 13     | Usa **RIGHT JOIN** para mantener datos derechos  | Devuelve todas las filas de la tabla derecha con coincidencias opcionales. |
| 19211619   | 14     | Implementa **FULL OUTER JOIN** para todas las filas | Muestra todas las filas de ambas tablas, con o sin coincidencias. |
| 22211548   | 15     | Usa **HAVING** para filtrar sobre agregados      | Filtra resultados después de aplicar funciones agregadas como SUM o AVG. |
| 20211765   | 16     | Simplifica agrupaciones con **ROLLUP**           | Calcula subtotales y totales jerárquicos en una sola consulta. |
| 22211555   | 17     | Usa **CUBE** para combinaciones totales          | Obtén todas las combinaciones posibles de agrupaciones y totales. |
| 22211562   | 18     | Aprovecha **UNION** para combinar resultados     | Une resultados de múltiples consultas eliminando duplicados automáticamente. |
| 22211567   | 19     | Usa **UNION ALL** para mantener duplicados       | Combina resultados de múltiples consultas sin eliminar duplicados. |
| 22211575   | 20     | Ordena con **ORDER BY** y múltiples criterios    | Define el orden de los resultados con una o más columnas. |
| 22211582   | 21     | Limita filas con **LIMIT** o **FETCH FIRST**     | Muestra un número específico de filas en los resultados. |
| 22211598   | 22     | Usa **OFFSET** para paginación                   | Salta un número de filas antes de devolver resultados. |
| 22211599   | 23     | Crea índices únicos con **UNIQUE**               | Asegura que los valores de una columna no se repitan en la tabla. |
| 22211600   | 24     | Usa **DEFAULT** para valores predefinidos        | Establece valores predeterminados para columnas al insertar datos. |
| 22211602   | 25     | Limpia datos con **TRIM**                        | Elimina espacios en blanco o caracteres específicos de cadenas. |
| 22211621   | 26     | Concatena cadenas con **CONCAT**                 | Une valores de texto en una sola columna o resultado. |
| 22211624   | 27     | Convierte valores con **CAST** y **CONVERT**     | Cambia tipos de datos entre texto, números o fechas. |
| 22211630   | 28     | Calcula fechas con **DATEDIFF** y **DATEADD**    | Resta o suma días entre fechas para análisis temporal. |
| 22210332   | 29     | Usa **EXTRACT** para obtener partes de una fecha | Extrae año, mes, día, hora, etc., de una columna de fecha. |
| 22211639   | 30     | Maneja zonas horarias con **AT TIME ZONE**       | Convierte fechas entre diferentes zonas horarias. |
| 22211648   | 31     | Usa **NOW()** o **CURRENT_DATE** para tiempo actual | Obtén la fecha y hora actuales del sistema en tus consultas. |
| 22211650   | 32     | Genera valores automáticos con **AUTO_INCREMENT** | Crea claves primarias únicas automáticamente al insertar datos. |
| 21211065   | 33     | Protege datos con **CHECK** en restricciones     | Asegura que los valores insertados cumplan ciertas condiciones. |
| 22211660   | 34     | Maneja claves foráneas con **FOREIGN KEY**       | Establece relaciones entre tablas con restricciones de integridad. |
| 24210765   | 35     | Usa **ON DELETE CASCADE** para eliminar en cascada | Borra datos relacionados automáticamente al eliminar una fila principal. |
| C20211465  | 36     | Evita duplicados con **INSERT IGNORE**           | Inserta filas solo si no hay conflictos con claves únicas. |
| 21212056   | 37     | Usa **REPLACE** para actualizar o insertar filas | Reemplaza filas existentes o las inserta si no existen. |
| 22211663   | 38     | Simplifica actualizaciones con **MERGE**         | Combina operaciones de `INSERT` y `UPDATE` en una sola sentencia. |
| 22211664   | 39     | Encuentra valores extremos con **MIN** y **MAX** | Obtén el valor más pequeño o grande de una columna numérica o de texto. |
| C21211400  | 40     | Cuenta filas con **COUNT(*)**                    | Devuelve el número total de filas en los resultados. |
| 22211665   | 41     | Usa **GROUP BY** para agrupaciones               | Organiza datos en grupos basados en una o más columnas. |
| C22211401  | 42     | Genera datos únicos con **ROW_NUMBER()**         | Asigna un número incremental a cada fila dentro de una partición. |
| 22211669   | 43     | Detecta duplicados con **DENSE_RANK()**          | Genera rankings consecutivos, incluso para valores repetidos. |
| 22211671   | 44     | Calcula promedios con **AVG**                    | Devuelve el promedio de los valores en una columna numérica. |
| 22210364   | 45     | Usa **SUM** para totales numéricos               | Suma los valores de una columna para grupos o filas. |

