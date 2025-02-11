# 🌍 Realizado por Ramirez Guerra 23210648

### 🌎 Problemas SQL sobre datos del mundo

En esta sección se presentan varios problemas y sus soluciones utilizando consultas SQL sobre una base de datos con información de países, sus continentes, población y PIB.

---

## 1️⃣. Mostrar los países con mayor población que Rusia 🏆
Este problema busca encontrar todos los países cuya población sea mayor que la de Rusia.

```sql
SELECT name FROM world
WHERE population > (SELECT population FROM world WHERE name LIKE 'Russia');
```

✅ **Solución:** Se usa una subconsulta para obtener la población de Rusia y se compara con los demás países.

---

## 2️⃣. Mostrar los países de Europa con mayor PIB per cápita que el Reino Unido 💶
Este problema busca los países europeos con un PIB per cápita mayor que el del Reino Unido.

```sql
SELECT name
FROM world
WHERE continent = 'Europe' AND
      gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom');
```

✅ **Solución:** Se calcula el PIB per cápita dividiendo el PIB entre la población y se usa una subconsulta para compararlo con el del Reino Unido.

---

## 3️⃣. Mostrar países que están en los continentes de Argentina y Australia 🌎
Este problema busca países que pertenezcan a los mismos continentes que Argentina o Australia.

```sql
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))
ORDER BY name;
```

✅ **Solución:** Se usa una subconsulta para obtener los continentes de Argentina y Australia y se filtran los países pertenecientes a ellos.

---

## 4️⃣. Mostrar países con población entre la del Reino Unido y Alemania 👥
Se listan los países cuya población sea mayor que la del Reino Unido, pero menor que la de Alemania.

```sql
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name LIKE 'United Kingdom')
AND population < (SELECT population FROM world WHERE name LIKE 'Germany');
```

✅ **Solución:** Se usa una doble subconsulta para establecer los límites de población entre los dos países.

---

## 5️⃣. Mostrar el porcentaje de población respecto a Alemania 🇩🇪
Se muestra el porcentaje de población de cada país de Europa en relación con la de Alemania.

```sql
SELECT name,  
       CONCAT(ROUND((population * 100.0) / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage  
FROM world  
WHERE continent = 'Europe';
```

✅ **Solución:** Se usa una subconsulta para obtener la población de Alemania y se calcula el porcentaje para cada país europeo.

---

## 6️⃣. Mostrar el país con mayor PIB del mundo 💰
Se busca el país con el mayor PIB del mundo.

```sql
SELECT name  
FROM world  
WHERE gdp > (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```

✅ **Solución:** Se usa `MAX()` para encontrar el PIB más alto de Europa y luego se listan los países que lo superan.

---

## 7️⃣. Mostrar el país con mayor área en cada continente 📏
Se obtiene el país con la mayor superficie en cada continente.

```sql
SELECT continent, name, area  
FROM world w1  
WHERE area = (SELECT MAX(area) FROM world w2 WHERE w1.continent = w2.continent);
```

✅ **Solución:** Se usa una subconsulta para encontrar el país con mayor área dentro de cada continente.

---

## 8️⃣. Mostrar el país con el nombre alfabéticamente menor en cada continente 🔠
Este problema encuentra el primer país en orden alfabético por continente.

```sql
SELECT continent, name
FROM world w1
WHERE name = (SELECT MIN(name) FROM world w2 WHERE w1.continent = w2.continent);
```

✅ **Solución:** Se usa `MIN(name)` para encontrar el país cuyo nombre es el primero alfabéticamente en cada continente.

---

## 9️⃣. Mostrar países en continentes con una población total menor a 25M 🌍
Este problema busca países que pertenezcan a continentes cuya población total sea menor o igual a 25 millones.

```sql
SELECT name, continent, population
FROM world w1
WHERE continent IN (
    SELECT continent
    FROM world w2
    GROUP BY continent
    HAVING MAX(population) <= 25000000);
```

✅ **Solución:** Se usa `HAVING` para filtrar los continentes con una población total menor o igual a 25M.

---

## 🔟. Mostrar países con más del triple de población que el segundo más poblado en su continente 👥
Se buscan países que tengan más del triple de población que el segundo país más poblado en su continente.

```sql
SELECT name, continent
FROM world w1
WHERE population > 3 * (
    SELECT MAX(population)
    FROM world w2
    WHERE w1.continent = w2.continent
    AND w2.name != w1.name
);
```


✅ **Solución:** Se usa una subconsulta para encontrar el segundo país más poblado de cada continente y se filtran aquellos cuya población sea más de tres veces mayor.
--- 
## Espero te sirvan estos codigos para practicar 
