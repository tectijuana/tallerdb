
# Consultas SQL Select De Select
## America Martinez Perez
**Número de control**: 21211991  
**Fecha**: 10 de febrero de 2025

---

### 1. Consulta: Poblaciones mayores a la de Rusia
```sql
SELECT name FROM world
WHERE population >
   (SELECT population FROM world
    WHERE name='Russia');
```
**Explicación**: Esta consulta selecciona los nombres de los países cuya población es mayor que la de Rusia.

---

### 2. Consulta: PIB per cápita mayor al del Reino Unido
```sql
SELECT name
FROM world
WHERE continent = 'Europe'
AND (gdp / population) > (
    SELECT gdp / population
    FROM world
    WHERE name = 'United Kingdom'
);
```
**Explicación**: Esta consulta selecciona los países de Europa cuyo PIB per cápita es mayor que el del Reino Unido.

---

### 3. Consulta: Países en el mismo continente que Argentina o Australia
```sql
SELECT name, continent
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    WHERE name IN ('Argentina', 'Australia')
)
ORDER BY name;
```
**Explicación**: Esta consulta selecciona los países que pertenecen al mismo continente que Argentina o Australia, ordenados por nombre.

---

### 4. Consulta: Población entre la del Reino Unido y Alemania
```sql
SELECT name, population
FROM world
WHERE population > (
    SELECT population FROM world WHERE name = 'United Kingdom'
)
AND population < (
    SELECT population FROM world WHERE name = 'Germany'
);
```
**Explicación**: Esta consulta selecciona los países cuya población es mayor que la del Reino Unido y menor que la de Alemania.

---

### 5. Consulta: Porcentaje de población de cada país europeo con respecto a Alemania
```sql
SELECT name, 
       CONCAT(ROUND((population * 100.0) / 
       (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage
FROM world
WHERE continent = 'Europe';
```
**Explicación**: Esta consulta calcula el porcentaje de la población de cada país europeo con respecto a la población de Alemania.

---

### 6. Consulta: Países con PIB mayor que todos los países de Europa
```sql
SELECT name
FROM world
WHERE gdp > ALL (
    SELECT gdp FROM world WHERE continent = 'Europe' AND gdp IS NOT NULL
);
```
**Explicación**: Esta consulta selecciona los países cuyo PIB es mayor que todos los países de Europa (excepto aquellos con un PIB nulo).

---

### 7. Consulta: Países con el área más grande de cada continente
```sql
SELECT continent, name, area
FROM world w1
WHERE area = (
    SELECT MAX(area) 
    FROM world w2 
    WHERE w1.continent = w2.continent
);
```
**Explicación**: Esta consulta selecciona el país con el área más grande dentro de cada continente.

---

### 8. Consulta: País con el nombre alfabéticamente más pequeño de cada continente
```sql
SELECT continent, name
FROM world w1
WHERE name = (
    SELECT MIN(name)
    FROM world w2
    WHERE w1.continent = w2.continent
);
```
**Explicación**: Esta consulta selecciona el país con el nombre más pequeño alfabéticamente dentro de cada continente.

---

### 9. Consulta: Países de continentes con población máxima menor o igual a 25 millones
```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000
);
```
**Explicación**: Esta consulta selecciona los países cuyos continentes tienen una población máxima menor o igual a 25 millones.

---

### 10. Consulta: Países con población más de tres veces mayor que el máximo en su continente
```sql
SELECT w1.name, w1.continent
FROM world w1
WHERE population > 3 * (
    SELECT MAX(w2.population)
    FROM world w2
    WHERE w2.continent = w1.continent
    AND w2.name != w1.name
);

**Explicación**: Esta consulta selecciona los países cuya población es más de tres veces mayor que la población de cualquier otro país en el mismo continente.
