## Realizado por Allyson Fernanda Monjaraz Torres 23210630

## 📌Sabias que❓
A través de SQL, los usuarios pueden realizar tareas como consultar, insertar, actualizar y eliminar datos en bases de datos. Además, SQL permite realizar análisis complejos, agregaciones y relacionar datos de múltiples tablas de forma eficiente.


### 1. Más grande que Rusia
Enumere el nombre de cada país cuya población sea mayor que la de Rusia.

**Solución:   
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```


### 2. Más rico que el Reino Unido
Mostrar los países de Europa con un PIB per cápita mayor que 'Reino Unido'.

**Solución:    
```sql
SELECT name FROM world
WHERE continent = 'Europe' 
AND gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom')
```

### 3. Vecinos de Argentina y Australia
Enumere el nombre y el continente de los países que contienen a Argentina o Australia . Ordene por nombre del país.

**Solución:    
```sql
SELECT name, continent FROM world
WHERE continent IN (SELECT continent FROM world WHERE name in ('Argentina',  'Australia'))
ORDER BY name
```

### 4. Entre Canadá y Polonia
¿Qué país tiene una población mayor que la del Reino Unido pero menor que la de Alemania? Indica el nombre y la población.

**Solución:    
```sql
SELECT name, population FROM world
WHERE population > (SELECT population FROM world WHERE name= 'United Kingdom') 
AND population < (SELECT population FROM world WHERE name = 'Germany')
```

### 5. Porcentajes de Alemania
Alemania (80 millones de habitantes) es el país más poblado de Europa. Austria (8,5 millones de habitantes) tiene el 11% de la población de Alemania.
Muestra el nombre y la población de cada país de Europa. Muestra la población como porcentaje de la población de Alemania.

**Solución:    
```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe'
```

### 6. Países cuyo PIB es mayor que el de todos los países de Europa
¿Qué países tienen un PIB mayor que todos los países de Europa? [Indique sólo el nombre .] (Algunos países pueden tener valores de PIB nulos)

**Solución:    
```sql
SELECT name FROM world
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe')
AND gdp IS NOT NULL
```

### 7. País más grande por área en cada Continente
¿Encuentra el país más grande (por área) en cada continente, muestra el continente , el nombre y el área :

**Solución:    
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area >0)
```

### 8.  Paises en orden alfabético
¿Enumere cada continente y el nombre del país que aparece primero en orden alfabético.

**Solución:    
```sql
SELECT continent , name
FROM world x
WHERE name= (SELECT MIN(name) FROM world y WHERE x.continent = y.continent)
```

### 9. Continentes donde su poblacion es menor que 25M 

**Solución:    
¿Encuentra los continentes donde todos los países tienen una población <= 25000000. Luego encuentra los nombres de los países asociados con estos continentes. Muestra el nombre, el continente y la población.
```sql
SELECT name, continent, population FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    GROUP BY continent
    HAVING MAX(population) <= 25000000
)
```

### 10. Paises 3 veces mas grande
¿Algunos países tienen una población tres veces mayor que la de todos sus vecinos (en el mismo continente). Mencione los países y los continentes.

**Solución:    
```sql
SELECT name, continent
FROM world x
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world y
    WHERE x.continent = y.continent
    AND y.name != x.name
);
```

## ⭐Aprendizaje: 
A través de estos ejercicios, hemos aprendido y practicado conceptos fundamentales y avanzados de SQL, aplicados a la consulta y análisis de datos en una base de datos de países.

**1️. Filtrado y comparaciones**
- Uso de `WHERE` para filtrar filas en una consulta, devolviendo solo aquellas que cumplen una condición específica.
- Comparaciones con subconsultas (  `>`, `<` ,`IN`, `MAX()`, `MIN()`).

**2️. Subconsultas y Funciones Agregadas**
- Uso de subconsultas dentro de `WHERE` 
- Funciones agregadas como `MAX()`, `MIN()`,`SUM()` se utilizan para realizar cálculos sobre un conjunto de datos y devolver un solo valor
- `HAVING` en combinación`GROUP BY` para filtrar resultados agregados, es decir, permite aplicar condiciones sobre los grupos creados.

**3️. Ordenación y Jerarquización de Datos**
- `ORDER BY` y `ROUND()` permiten organizar y formatear los datos de manera más eficiente, mejorando la presentación y el análisis de los resultados.

**4️. Uso de `IN` y `JOIN` Implícito**
- Uso `IN`se utiliza para filtrar filas que coinciden con varios valores en una columna, sin necesidad de usar múltiples condiciones `OR`
- Un `JOIN` implícito (también conocido como `INNER JOIN` implícito) se utiliza para combinar datos de dos o más tablas basándose en una relación entre ellas.
- Identificación de países dominantes dentro de sus continentes mediante comparaciones con `SUM()`.
