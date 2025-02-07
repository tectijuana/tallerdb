
# SELECT from WORLD Tutorial

## 1. Introducción
Muestra el `name`, `continent` y `population` de todos los países.

```sql
SELECT name, continent, population FROM world
```

<details>
    <summary>Resultado:</summary>

| Name                           | Continent       | Population    |
|--------------------------------|-----------------|---------------|
| Afghanistan                    | Asia            | 25,500,100    |
| Albania                        | Europe          | 2,821,977     |
| Algeria                        | Africa          | 38,700,000    |
| Andorra                        | Europe          | 76,098        |
| Angola                         | Africa          | 19,183,590    |
| Antigua and Barbuda            | Caribbean       | 86,295        |
| Argentina                      | South America   | 42,669,500    |
| Armenia                        | Eurasia         | 3,017,400     |
| Australia                      | Oceania         | 23,545,500    |
| Austria                        | Europe          | 8,504,850     |
| Azerbaijan                     | Asia            | 9,477,100     |
| Bahamas                        | Caribbean       | 351,461       |
| Bahrain                        | Asia            | 1,234,571     |
| Bangladesh                     | Asia            | 156,557,000   |
| Barbados                       | Caribbean       | 285,000       |
| Belarus                        | Europe          | 9,467,000     |
| Belgium                        | Europe          | 11,198,638    |
| Belize                         | North America   | 349,728       |
| Benin                          | Africa          | 9,988,068     |
| Bhutan                         | Asia            | 749,090       |
| Bolivia                        | South America   | 10,027,254    |
| Bosnia and Herzegovina         | Europe          | 3,791,622     |
| Botswana                       | Africa          | 2,024,904     |
| Brazil                         | South America   | 202,794,000   |
| Brunei                         | Asia            | 393,162       |
| Bulgaria                       | Europe          | 7,245,677     |
| Burkina Faso                   | Africa          | 17,322,796    |
| Burundi                        | Africa          | 9,420,248     |
| Cambodia                       | Asia            | 15,184,116    |
| Cameroon                       | Africa          | 20,386,799    |
| Canada                         | North America   | 35,427,524    |
| Cape Verde                     | Africa          | 491,875       |
| Côte d'Ivoire                  | Africa          | 23,919,000    |
| Central African Republic       | Africa          | 4,709,000     |
| Chad                           | Africa          | 13,211,000    |
| Chile                          | South America   | 17,773,000    |
| China                          | Asia            | 1,365,370,000 |
| Colombia                       | South America   | 47,662,000    |
| Comoros                        | Africa          | 743,798       |
| Congo, Democratic Republic of  | Africa          | 69,360,000    |
| Congo, Republic of             | Africa          | 4,559,000     |
| Costa Rica                     | North America   | 4,667,096     |
| Croatia                        | Europe          | 4,290,612     |
| Cuba                           | Caribbean       | 11,167,325    |
| Cyprus                         | Asia            | 865,878       |
| Czech Republic                 | Europe          | 10,517,400    |
| Denmark                        | Europe          | 5,634,437     |
| Djibouti                       | Africa          | 886,000       |
| Dominica                       | Caribbean       | 71,293        |
| Dominican Republic             | Caribbean       | 9,445,281     |

</details>

## 2. Países grandes
Cómo usar `WHERE` para filtrar registros. Muestra el `name` de los países que tienen una población de al menos 200 millones. 200 millones es 200000000, hay ocho ceros.

```sql
SELECT name FROM world
WHERE population >= 200000000
```

<details>
    <summary>Resultado:</summary>

| Name          |
|---------------|
| Brazil        |
| China         |
| India         |
| Indonesia     |
| United States |
</details>

## 3. PIB per cápita
Muestra el `name` y el PIB per cápita para aquellos países con una población de al menos 200 millones.

```sql
SELECT name, gdp/population FROM world
WHERE population >= 200000000
```

## 4. Sudamérica en millones
Muestra el `name` y la `population` en millones para los países del continente 'South America'. Divide la población por 1000000 para obtener la población en millones.

```sql
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
```

<details>
  <summary>Resultado:</summary>

| Name                                  | Population (millones) |
|---------------------------------------|----------------------|
| Argentina                             | 42.6695              |
| Bolivia                               | 10.0273              |
| Brazil                                | 202.7940             |
| Chile                                 | 17.7730              |
| Colombia                              | 47.6620              |
| Ecuador                               | 15.7742              |
| Guyana                                | 0.7849               |
| Paraguay                              | 6.7834               |
| Peru                                  | 30.4751              |
| Saint Vincent and the Grenadines      | 0.1090               |
| Suriname                              | 0.5342               |
| Uruguay                               | 3.2863               |
| Venezuela                             | 28.9461              |
</details>

## 5. Francia, Alemania, Italia
Muestra el `name` y la `population` de Francia, Alemania e Italia.

```sql
SELECT name, population FROM world
WHERE name in ('France', 'Germany', 'Italy')
```

<details>
  <summary>Resultado:</summary>

| Name    | Population  |
|---------|-------------|
| France  | 65,906,000  |
| Germany | 80,716,000  |
| Italy   | 60,782,668  |
</details>

## 6. United
Muestra los países cuyo `name` contiene la palabra 'United'.

```sql
SELECT name FROM world
WHERE name LIKE '%United%'
```

<details>
  <summary>Resultado:</summary>

| Name                  |
|-----------------------|
| United Arab Emirates  |
| United Kingdom        |
| United States         |
</details>

## 7. Dos formas de ser grande
Dos formas de ser grande: Un país es **grande** si tiene un área de más de 3 millones de km² o si tiene una población de más de 250 millones.

```sql
SELECT name, population, area FROM world
WHERE area >= 3000000 OR population >= 250000000
```

<details>
  <summary>Resultado:</summary>

| Name         | Population   | Area (sq km)  |
|--------------|--------------|---------------|
| Australia    | 23,545,500   | 7,692,024     |
| Brazil       | 202,794,000  | 8,515,767     |
| Canada       | 35,427,524   | 9,984,670     |
| China        | 1,365,370,000| 9,596,961     |
| India        | 1,246,160,000| 3,166,414     |
| Indonesia    | 252,164,800  | 1,904,569     |
| Russia       | 146,000,000  | 17,125,242    |
| United States| 318,320,000  | 9,826,675     |
</details>

## 8. Uno u otro (pero no ambos)
OR exclusivo (XOR). Muestra los países que son grandes por área (más de 3 millones de km²) o grandes por población (más de 250 millones), pero no ambos. Muestra `name`, `population` y `area`.

```sql
SELECT name, population, area FROM world
WHERE (area >= 3000000 OR population >= 250000000)
AND NOT (area >= 3000000 AND population >= 250000000)
```

<details>
  <summary>Resultado:</summary>

| Name       | Population   | Area (sq km)  |
|-----------|-------------:|--------------:|
| Australia | 23,545,500   | 7,692,024     |
| Brazil    | 202,794,000  | 8,515,767     |
| Canada    | 35,427,524   | 9,984,670     |
| Indonesia | 252,164,800  | 1,904,569     |
| Russia    | 146,000,000  | 17,125,242    |
</details>

## 9. Redondeo
Muestra el `name` y la `population` en millones y el PIB en miles de millones para los países del continente 'South America'. Usa la función `ROUND()` para mostrar los valores con dos decimales.

```sql
SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2) FROM world
WHERE continent = 'South America'
```

<details>
  <summary>Resultado:</summary>

| Name                              | Population (millions) | GDP (billions) |
|-----------------------------------|-----------------------|----------------|
| Argentina                         | 42.67                 | 477.03         |
| Bolivia                           | 10.03                 | 27.04          |
| Brazil                            | 202.79                | 2,254.11       |
| Chile                             | 17.77                 | 268.31         |
| Colombia                          | 47.66                 | 369.81         |
| Ecuador                           | 15.77                 | 87.50          |
| Guyana                            | 0.78                  | 2.85           |
| Paraguay                          | 6.78                  | 25.94          |
| Peru                              | 30.48                 | 204.68         |
| Saint Vincent and the Grenadines  | 0.11                  | 0.69           |
| Suriname                          | 0.53                  | 5.01           |
| Uruguay                           | 3.29                  | 49.92          |
| Venezuela                         | 28.95                 | 382.42         |
</details>

## 10. Economías de un billón de dólares
Muestra el `name` y el PIB per cápita para aquellos países con un PIB de al menos un billón (1000000000000; es decir, 12 ceros). Redondea este valor a la centena más cercana.

```sql
SELECT name, ROUND(gdp/population, -3) FROM world
WHERE gdp >= 1000000000000
```

<details>
  <summary>Resultado:</summary>

| Name            | GDP per capita |
|-----------------|----------------|
| Australia       | 66,000         |
| Brazil          | 11,000         |
| Canada          | 45,000         |
| China           | 6,000          |
| France          | 40,000         |
| Germany         | 42,000         |
| India           | 2,000          |
| Italy           | 33,000         |
| Japan           | 47,000         |
| Mexico          | 10,000         |
| Russia          | 14,000         |
| South Korea     | 22,000         |
| Spain           | 28,000         |
| United Kingdom  | 39,000         |
| United States   | 51,000         |
</details>

## 11. Nombre y capital con la misma longitud
Grecia tiene como capital Atenas.
Tanto 'Greece' como 'Athens' tienen 6 caracteres.
Muestra el `name` y `capital` donde el nombre y la capital tienen la misma cantidad de caracteres.

```sql
SELECT name, capital FROM world
WHERE LENGTH(name) = LENGTH(capital)
```

<details>
  <summary>Resultado:</summary>

| Name        | Capital      |
|-------------|--------------|
| Algeria     | Algiers      |
| Angola      | Luanda       |
| Armenia     | Yerevan      |
| Botswana    | Gaborone     |
| Canada      | Ottowa       |
| Djibouti    | Djibouti     |
| Egypt       | Cairo        |
| Estonia     | Tallinn      |
| Fiji        | Suva         |
| Gambia      | Banjul       |
| Georgia     | Tbilisi      |
| Ghana       | Accra        |
| Greece      | Athens       |
| Luxembourg  | Luxembourg   |
| Mauritania  | Nouakchott   |
| Peru        | Lima         |
| Poland      | Warsaw       |
| Russia      | Moscow       |
| Rwanda      | Kigali       |
| San Marino  | San Marino   |
| Singapore   | Singapore    |
| Taiwan      | Taipei       |
| Turkey      | Ankara       |
| Zambia      | Lusaka       |
</details>

## 12. Nombre y capital coincidentes
Muestra el `name` y `capital` donde la primera letra de ambos coincida. No incluyas países donde el nombre y la capital sean la misma palabra.

```sql
SELECT name, capital FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1) AND name <> capital
```

<details>
  <summary>Resultado:</summary>

| Name                  | Capital                       |
|-----------------------|-------------------------------|
| Algeria               | Algiers                       |
| Andorra               | Andorra la Vella              |
| Barbados              | Bridgetown                    |
| Belize                | Belmopan                       |
| Brazil                | Brasília                      |
| Brunei                | Bandar Seri Begawan           |
| Burundi               | Bujumbura                     |
| Guatemala             | Guatemala City                |
| Guyana                | Georgetown                     |
| Kuwait                | Kuwait City                   |
| Maldives              | Malé                          |
| Marshall Islands      | Majuro                        |
| Mexico                | Mexico City                   |
| Monaco                | Monaco-Ville                  |
| Mozambique            | Maputo                        |
| Niger                 | Niamey                        |
| Panama                | Panama City                   |
| Papua New Guinea      | Port Moresby                  |
| São Tomé and Príncipe | São Tomé                      |
| South Korea           | Seoul                         |
| Sri Lanka             | Sri Jayawardenepura Kotte     |
| Sweden                | Stockholm                     |
| Taiwan                | Taipei                        |
| Tunisia               | Tunis                         |
</details>

## 13. Todas las vocales
Encuentra el país que tiene todas las vocales y no contiene espacios en su nombre.

```sql
SELECT name FROM world
WHERE name NOT LIKE '% %'
  AND name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
```

<details>
  <summary>Resultado:</summary>

| Name        |
|-------------|
| Mozambique  |
</details>

