---
## Hecho por Ulises Hernández Bojórquez - 2320598
---
# 📊 Consultas SQL Avanzadas

Este documento presenta una serie de consultas SQL diseñadas para responder preguntas complejas sobre una base de datos que almacena información de países en una tabla llamada `world`. Se incluyen consultas con subconsultas, comparaciones entre países y operaciones en grupos de datos.

---

## 🌍 1. Países con mayor población que Rusia

**Descripción:** Esta consulta lista los nombres de los países cuya población es mayor que la de Rusia.

```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```

---

## 💰 2. Países de Europa con un PIB per cápita mayor que el del Reino Unido

**Descripción:** Muestra los países europeos donde el PIB per cápita es superior al del Reino Unido.

```sql
SELECT name
FROM world
WHERE gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom')
AND continent = 'Europe';
```

---

## 🌎 3. Países en los continentes de Argentina o Australia

**Descripción:** Lista el nombre y el continente de los países que pertenecen a los continentes donde se encuentran Argentina o Australia. Se ordena por nombre.

```sql
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Australia', 'Argentina'))
ORDER BY name;
```

---

## 📈 4. Países con población entre la de Reino Unido y Alemania

**Descripción:** Muestra los países cuya población es mayor que la del Reino Unido pero menor que la de Alemania.

```sql
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom')
AND population < (SELECT population FROM world WHERE name = 'Germany');
```

---

## 🏢 5. Población de cada país de Europa como porcentaje de la población de Alemania

**Descripción:** Se calcula el porcentaje de la población de cada país en Europa en relación con la población de Alemania.

```sql
SELECT name, CONCAT(CAST(ROUND(100 * population / (SELECT population FROM world WHERE name = 'Germany'), 0) AS INT), '%') AS population_percentage
FROM world
WHERE continent = 'Europe';
```

---

## 🌎 6. Países con un PIB mayor que cualquier país de Europa

**Descripción:** Se listan los países cuyo PIB es mayor que el de cualquier país en Europa. Se excluyen los valores `NULL`.

```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```

---

## 🏆 7. País más grande (por área) en cada continente

**Descripción:** Encuentra el país con mayor superficie en cada continente utilizando una subconsulta correlacionada.

```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent = x.continent
          AND area > 0);
```

---

## 🔤 8. Primer país alfabéticamente por continente

**Descripción:** Lista el primer país en orden alfabético en cada continente.

```sql
SELECT continent, name
FROM world x
WHERE name = (SELECT MIN(name) FROM world y WHERE x.continent = y.continent);
```

---

## 🌍 9. Continentes donde todos los países tienen una población ≤ 25,000,000

**Descripción:** Encuentra los continentes donde ningún país tiene una población mayor a 25 millones, y luego muestra los países dentro de esos continentes.

```sql
SELECT name, continent, population 
FROM world
WHERE continent IN (
    SELECT continent
    FROM world
    GROUP BY continent
    HAVING MAX(population) <= 25000000);
```

---

## 🌟 10. Países con población más de tres veces la de cualquier vecino

**Descripción:** Encuentra países cuya población es más de tres veces la de cualquier otro país en su mismo continente.

```sql
SELECT name, continent
FROM world x
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world y
    WHERE x.continent = y.continent
    AND y.name != x.name);
```

---

## 🎯 Conclusión

Estas consultas ilustran diversas técnicas avanzadas de SQL, incluyendo subconsultas correlacionadas, comparaciones con agregaciones y filtrado basado en múltiples condiciones. Son útiles para analizar datos poblacionales y económicos de manera efectiva. 🚀

