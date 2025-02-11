
# SELECT within SELECT

## 1. Más grande que Rusia
Lista cada país cuyo `population` es mayor que el de 'Russia'.

```sql
SELECT name FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

<details>
    <summary>Resultado:</summary>

| Name          |
|---------------|
| Bangladesh    |
| Brazil        |
| China         |
| India         |
| Indonesia     |
| Nigeria       |
| Pakistan      |
| United States |
</details>

## 2. Más rico que el Reino Unido
Muestra los países en `Europe` con un PIB per cápita mayor que el de 'United Kingdom'.

```sql
SELECT name FROM world
WHERE continent='Europe' AND gdp/population >
     (SELECT gdp/population FROM world
      WHERE name='United Kingdom')
```

<details>
    <summary>Resultado:</summary>

| Name         |
|--------------|
| Andorra      |
| Austria      |
| Belgium      |
| Denmark      |
| Finland      |
| France       |
| Germany      |
| Iceland      |
| Ireland      |
| Liechtenstein|
| Luxembourg   |
| Monaco       |
| Netherlands  |
| Norway       |
| San Marino   |
| Sweden       |
| Switzerland  |
</details>

## 3. Vecinos de Argentina y Australia

Lista el `name` y `continent` de los países en los continentes que contienen a Argentina o Australia. Ordena por el nombre del país.

```sql
SELECT name, continent FROM world
WHERE continent IN
     (SELECT continent FROM world
      WHERE name='Argentina' OR name='Australia')
ORDER BY name ASC
```

<details>
  <summary>Resultado:</summary>

| Name                                 | Continent      |
|--------------------------------------|----------------|
| Argentina                            | South America  |
| Australia                            | Oceania        |
| Bolivia                              | South America  |
| Brazil                               | South America  |
| Chile                                | South America  |
| Colombia                             | South America  |
| Ecuador                              | South America  |
| Fiji                                 | Oceania        |
| Guyana                               | South America  |
| Kiribati                             | Oceania        |
| Marshall Islands                     | Oceania        |
| Micronesia, Federated States of      | Oceania        |
| Nauru                                | Oceania        |
| New Zealand                          | Oceania        |
| Palau                                | Oceania        |
| Papua New Guinea                     | Oceania        |
| Paraguay                             | South America  |
| Peru                                 | South America  |
| Saint Vincent and the Grenadines     | South America  |
| Samoa                                | Oceania        |
| Solomon Islands                      | Oceania        |
| Suriname                             | South America  |
| Tonga                                | Oceania        |
| Tuvalu                               | Oceania        |
| Uruguay                              | South America  |
| Vanuatu                              | Oceania        |
| Venezuela                            | South America  |
</details>

## 4. Entre Canadá y Polonia

¿Qué país tiene una población mayor que la del Reino Unido pero menor que la de Alemania? Muestra el `name` y la `population`.

```sql
SELECT name, population FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name='United Kingdom')
  AND population <
     (SELECT population FROM world
      WHERE name='Germany')
```

<details>
  <summary>Resultado:</summary>

| Name                           | Population |
|--------------------------------|------------|
| Congo, Democratic Republic of  | 69,360,000 |
| France                         | 65,906,000 |
| Iran                           | 77,552,000 |
| Thailand                       | 64,456,700 |
| Turkey                         | 76,667,864 |
</details>

## 5. Porcentajes de Alemania

Muestra el `name` y la `population` de cada país en Europa. Muestra la población como un porcentaje de la población de Alemania.

```sql
SELECT name, CONCAT(ROUND(100*population/(SELECT population FROM world WHERE name = 'Germany')), '%') FROM world
WHERE continent = 'Europe'
```

<details>
  <summary>Resultado:</summary>

| Name                           | Population (%)|
|--------------------------------|---------------|
| Albania                        | 3%            |
| Andorra                        | 0%            |
| Austria                        | 11%           |
| Belarus                        | 12%           |
| Belgium                        | 14%           |
| Bosnia and Herzegovina         | 5%            |
| Bulgaria                       | 9%            |
| Croatia                        | 5%            |
| Czech Republic                 | 13%           |
| Denmark                        | 7%            |
| Estonia                        | 2%            |
| Finland                        | 7%            |
| France                         | 82%           |
| Germany                        | 100%          |
| Greece                         | 14%           |
| Hungary                        | 12%           |
| Iceland                        | 0%            |
| Ireland                        | 6%            |
| Italy                          | 75%           |
| Kazakhstan                     | 21%           |
| Latvia                         | 2%            |
| Liechtenstein                  | 0%            |
| Lithuania                      | 4%            |
| Luxembourg                     | 1%            |
| Macedonia                      | 3%            |
| Malta                          | 1%            |
| Moldova                        | 4%            |
| Monaco                         | 0%            |
| Montenegro                     | 1%            |
| Netherlands                    | 21%           |
| Norway                         | 6%            |
| Poland                         | 48%           |
| Portugal                       | 13%           |
| Romania                        | 25%           |
| San Marino                     | 0%            |
| Serbia                         | 9%            |
| Slovakia                       | 7%            |
| Slovenia                       | 3%            |
| Spain                          | 58%           |
| Sweden                         | 12%           |
| Switzerland                    | 10%           |
| Ukraine                        | 53%           |
| United Kingdom                 | 79%           |
| Vatican City                   | 0%            |
</details>

## 6. Más grande que cualquier país de Europa

¿Qué países tienen un PIB mayor que cualquier país de Europa? (Algunos países pueden tener valores de PIB NULL). Muestra solo el `name`.

```sql
SELECT name FROM world
WHERE gdp IS NOT NULL
  AND gdp > ALL(SELECT gdp FROM world
                      WHERE continent = 'Europe' and gdp IS NOT NULL)
```

<details>
  <summary>Resultado:</summary>

| Name          |
|---------------|
| China         |
| Japan         |
| United States |
</details>

## 7. El más grande de cada continente

Encuentra el país más grande (por área) en cada continente. Muestra `continent`, `name` y `area`.

```sql
SELECT continent, name, area FROM world x
WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent)
```

<details>
  <summary>Resultado:</summary>

| Continent     | Name       | Area       |
|---------------|------------|------------|
| Africa        | Algeria    | 2,381,741  |
| Oceania       | Australia  | 7,692,024  |
| South America | Brazil     | 8,515,767  |
| North America | Canada     | 9,984,670  |
| Asia          | China      | 9,596,961  |
| Caribbean     | Cuba       | 109,884    |
| Europe        | Kazakhstan | 2,724,900  |
| Eurasia       | Russia     | 17,125,242 |
</details>

## 8. Primer país de cada continente (alfabéticamente)

Lista cada continente y el nombre del país que aparece primero en orden alfabético.

```sql
SELECT continent, name FROM world x
WHERE name <= ALL
    (SELECT name FROM world y
        WHERE y.continent=x.continent)
```

<details>
  <summary>Resultado:</summary>

| Continent    | Name                  |
|--------------|-----------------------|
| Africa       | Algeria               |
| Asia         | Afghanistan           |
| Caribbean    | Antigua and Barbuda   |
| Eurasia      | Armenia               |
| Europe       | Albania               |
| North America| Belize                |
| Oceania      | Australia             |
| South America| Argentina             |
</details>

## 9. Preguntas difíciles que utilizan técnicas no cubiertas en secciones anteriores.

Encuentra los continentes donde todos los países tienen una población menor o igual a 25 millones. Luego, muestra los nombres de los países asociados a estos continentes. Muestra `name`, `continent` y `population`.

```sql
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(SELECT population FROM world y
                      WHERE x.continent = y.continent)
```

<details>
  <summary>Resultado:</summary>

| Name                           | Continent  | Population |
|--------------------------------|------------|------------|
| Antigua and Barbuda            | Caribbean  | 86,295     |
| Australia                      | Oceania    | 23,545,500 |
| Bahamas                        | Caribbean  | 351,461    |
| Barbados                       | Caribbean  | 285,000    |
| Cuba                           | Caribbean  | 11,167,325 |
| Dominica                       | Caribbean  | 71,293     |
| Dominican Republic             | Caribbean  | 9,445,281  |
| Fiji                           | Oceania    | 858,038    |
| Grenada                        | Caribbean  | 103,328    |
| Haiti                          | Caribbean  | 10,413,211 |
| Jamaica                        | Caribbean  | 2,717,991  |
| Kiribati                       | Oceania    | 106,461    |
| Marshall Islands               | Oceania    | 56,086     |
| Micronesia, Federated States of| Oceania    | 101,351    |
| Nauru                          | Oceania    | 9,945      |
| New Zealand                    | Oceania    | 4,538,520  |
| Palau                          | Oceania    | 20,901     |
| Papua New Guinea               | Oceania    | 7,398,500  |
| Saint Lucia                    | Caribbean  | 180,000    |
| Samoa                          | Oceania    | 187,820    |
| Solomon Islands                | Oceania    | 581,344    |
| Tonga                          | Oceania    | 103,036    |
| Trinidad and Tobago            | Caribbean  | 1,328,019  |
| Tuvalu                         | Oceania    | 11,323     |
| Vanuatu                        | Oceania    | 264,652    |
</details>

## 10. Tres veces más grande

```sql
SELECT name, continent FROM world x
WHERE population >= ALL(SELECT population*3 FROM world y
                      WHERE x.continent = y.continent
                        AND y.name <> x.name)
```

<details>
  <summary>Resultado:</summary>

| Name      | Continent     |
|-----------|---------------|
| Australia | Oceania       |
| Brazil    | South America |
| Russia    | Eurasia       |
</details>

