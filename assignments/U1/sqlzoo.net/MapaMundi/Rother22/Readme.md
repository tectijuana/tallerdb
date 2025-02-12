---

### **Ulises Hernandez Bojorquez - #23210598**  

---

# **Consultas de la pratica de clase en SQLZOO 🚀**  
## Practica de clase 👍

### **1. Mostrar nombre, continente y población de todos los países**  
```sql
SELECT name, continent, population FROM world;
```

### **2. Filtrar países con una población de al menos 200 millones**  
```sql
SELECT name 
FROM world
WHERE population >= 200000000;
```

### **3. Mostrar nombre y PIB per cápita para países con al menos 200 millones de habitantes**  
```sql
SELECT name, gdp/population AS per_capita_gdp
FROM world
WHERE population >= 200000000;
```

### **4. Mostrar el nombre y la población en millones para los países de Sudamérica**  
```sql
SELECT name, population / 1000000 AS population_millions
FROM world
WHERE continent = 'South America';
```

### **5. Mostrar el nombre y la población de Francia, Alemania e Italia**  
```sql
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```

### **6. Mostrar los países cuyo nombre contiene la palabra 'United'**  
```sql
SELECT name
FROM world
WHERE name LIKE '%United%';
```

### **7. Mostrar los países que son grandes por área (más de 3 millones km²) o por población (más de 250 millones)**  
```sql
SELECT name, population, area
FROM world
WHERE area > 3000000
   OR population > 250000000;
```

### **8. Mostrar países que son grandes por área o por población, pero no por ambas (XOR)**  
```sql
SELECT name, population, area
FROM world
WHERE 
    (population > 250000000 AND area < 3000000)
    OR 
    (population < 250000000 AND area > 3000000);
```

### **9. Mostrar nombre, población en millones y PIB en miles de millones para Sudamérica y América**  
```sql
SELECT name, 
       ROUND(population / 1000000, 2) AS population_millions, 
       ROUND(gdp / 1000000000, 2) AS gdp_billions
FROM world
WHERE continent = 'South America';
```

### **10. Mostrar nombre y PIB per cápita para países con un PIB de al menos 1 billón, redondeado a 1000**  
```sql
SELECT name, 
       ROUND(gdp / population, -3) AS per_capita_gdp
FROM world
WHERE gdp >= 1000000000000;
```

### **11. Mostrar países donde el nombre y la capital tienen la misma cantidad de caracteres**  
```sql
SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital);
```

### **12. Mostrar países donde el nombre y la capital comienzan con la misma letra (pero no son iguales)**  
```sql
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1) 
  AND name != capital;
```

### **13. Encontrar el país cuyo nombre contiene todas las vocales (a, e, i, o, u) sin espacios**  
```sql
SELECT name 
FROM world
WHERE name LIKE '%a%' 
  AND name LIKE '%e%' 
  AND name LIKE '%i%' 
  AND name LIKE '%o%' 
  AND name LIKE '%u%' 
  AND name NOT LIKE '% %';
```














