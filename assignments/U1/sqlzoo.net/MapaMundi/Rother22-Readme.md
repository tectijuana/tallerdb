### 

1. Read the notes about this table. Observe the result of running this SQL command to show the name, continent and population of all countries.

,,,SQL
SELECT name, continent, population FROM world
,,,

2.How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros. 
SELECT name FROM world
WHERE population >= 200000000

3.Give the name and the per capita GDP for those countries with a population of at least 200 million. 
Select name, gdp/population
From world
Where population >= 200000000

4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
Select name, population/1000000 
From world
Where continent = 'south america'

5. Show the name and population for France, Germany, Italy
   Select name, population
From world
Where name in ('France','Germany','Italy')

6.Show the countries which have a name that includes the word 'United'
Select name
From world
Where name Like '%United%'

7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.

Show the countries that are big by area or big by population. Show name, population and area.

Select name, population, area
From world
Where area > 3000000
or population >250000000

8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.

    Australia has a big area but a small population, it should be included.
    Indonesia has a big population but a small area, it should be included.
    China has a big population and big area, it should be excluded.
    United Kingdom has a small population and a small area, it should be excluded.

Select name, population, area
From world
Where 
(population > 250000000 and area < 3000000)
or
(population < 250000000 and area > 3000000);


9.


























