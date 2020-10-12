# SQLZoo-answers
My answers for [SQLZoo](https://sqlzoo.net) tutorials questions 

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL (harder questions)](#select-from-nobel)
4. [NESTED SELECT](#nested-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT basics
<img src="img/1 Stage - Table world.png"/>
Some simple queries to get you started

1.1 Modify it to show the population of Germany
```sql
  SELECT population FROM world
    WHERE name = 'Germany'
```
1.2 Modify it to show the population of Russia
```sql
  SELECT population FROM world
    WHERE name = 'Russia'
```

2.1 Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
```sql
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark');
```
2.2 Show the name and the population for 'Brazil', 'Russia', 'India' and 'China'.
```sql
SELECT name, population FROM world
  WHERE name IN ('Brazil', 'Russia', 'India', 'China');
```

3.1 Shows countries with an area of 250,000-300,000 sq. km.
```sql
SELECT name, area FROM world
  WHERE area BETWEEN 250000 AND 300000
```
3.2 Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```

## SELECT from WORLD
1.1 Show the name, continent and population of all countries.
```sql
SELECT name, continent, population FROM world
```
1.2 Show the name and population with limit 5 countries.
```sql
SELECT name, population FROM world
  LIMIT 5
```
2.1 Show the name for the countries that have a population of at least 200,000,000 (200 million).
```sql
SELECT name FROM world
  WHERE population >= 200000000
```
2.2 Show the name and population for the countries that have a population of at less 1,000,000 (1 million).
```sql
SELECT name, population FROM world
  WHERE population <= 1000
```
3. Give the name and the per capita GDP for those countries with a population of at least 200 million.
```sql
SELECT name, gdp/population 
FROM world
WHERE population > 200000000
```
4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
SELECT name, population/1000000 
FROM world
WHERE continent = 'South America'
```
5. Show the name and population for France, Germany, Italy
```sql
SELECT name, population 
FROM world
WHERE name IN ('France', 'Germany', 'Italy')
```
6.1 Show the countries which have a name that includes the word 'United'
```sql
SELECT name
FROM world
WHERE name LIKE 'United%'
```
6.2 Show name, continent for all countries which have a name continent equals 'Asia'
```sql
SELECT name, continent
FROM world
WHERE continent = 'Asia'
```
6.3 Show unique continent from table world, and order by continent
```sql
SELECT DISTINCT continent
FROM world
ORDER BY continent
```
7. Show the countries that are big by area OR big by population. Show name, population and area.
```sql
SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000
```
8. Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. 
   <p>Show name, population and area. Exclusive OR (XOR).</p>
```sql
SELECT name, population, area
FROM world
WHERE area > 3000000 XOR population > 250000000
```
9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. 
   Use the ROUND function to show the values to two decimal places.
```sql
SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2)
FROM world
WHERE continent = 'South America'
```
10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). 
    Round this value to the nearest 1000.
```sql
SELECT name, ROUND(gdp/population, -3)
FROM world
WHERE gdp > 1000000000000
```
11.1 Show the name and capital where the name and the capital have the same number of characters.
	You can use the LENGTH function to find the number of characters in a string
```sql
SELECT name, capital 
FROM world
WHERE LENGTH(name) = LENGTH(capital )
```
11.2 Show the name, continent and capital and number of characters, where name countries like 'G%'
```sql
SELECT name,      LENGTH(name),
       continent, LENGTH(continent),
       capital, LENGTH(capital)
  FROM world
 WHERE name LIKE 'G%'
```
12. Show the name and the capital where the first letters of each match. 
    Don't include countries where the name and the capital are the same word.
	You can use the function LEFT to isolate the first character.
    You can use <> (or !=) as the NOT EQUALS operator.
```sql
SELECT name, capital
FROM world
WHERE LEFT(name,1) = LEFT(capital,1) 
  AND name != capital
```
13. Find the country that has all the vowels (a e i o u) and no spaces in its name.
	You can use the phrase name NOT LIKE '%a%' to exclude characters from your results.
```sql
SELECT name
FROM world
WHERE name NOT LIKE '% %'
  AND name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
```

## SELECT from NOBEL (harder questions)
<img src="img/3 Stage - Tabel Nobel Laureates.png"/>

1.1 Shown so that it displays Nobel prizes for 1950.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```
1.2 Show all winners Nobel laureates from 1950-1952 years, and order it's by years.
SELECT yr, subject, winner
FROM nobel
WHERE yr BETWEEN 1950 AND 1952
ORDER BY yr
1.3 Show unique subject from Nobel tables
```sql
SELECT DISTINCT subject
FROM nobel
```
2.1 Show who won the 1962 prize for Literature.
```sql
SELECT winner
FROM nobel
WHERE subject = 'Literature'
AND yr = 1962
```
2.2 Show who won the 1960 prize for Physics.
```sql
SELECT winner
FROM nobel
WHERE yr = 1960
AND subject = 'Physics'
```
3. Show the year and subject that won 'Albert Einstein' his prize.
```sql
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
```
4. Give the name of the 'Peace' winners since the year 2000, including 2000.
```sql
SELECT winner
FROM nobel
WHERE subject = 'Peace'
AND yr >= 2000
```
5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
```sql
SELECT * (yr, subject, winner -> another variant)
FROM nobel
WHERE subject = 'Literature '
AND yr BETWEEN 1980 AND 1989
```
6. Show all details of the presidential winners:
- Theodore Roosevelt, 
- Woodrow Wilson, 
- Jimmy Carter,
- Barack Obama
```sql
SELECT yr, subject, winner
FROM nobel
WHERE winner IN ('Theodore Roosevelt','Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
```
7.1 Show the winners with first name John
```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John%'
```
7.2 Show the winners with name consist 'stian'
```sql
SELECT winner
FROM nobel
WHERE winner LIKE '%stian%'
```
8 Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE (subject ='Physics' AND yr = 1980)
OR (subject = 'Chemistry' AND yr = 1984)
```
9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine
```sql
SELECT *
FROM nobel
WHERE subject NOT IN ('Chemistry', 'Medicine')
AND yr = 1980
```
10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) 
together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
SELECT *
FROM nobel
WHERE subject = 'Medicine' AND yr < 1910
OR subject = 'Literature' AND yr >= 2004
```
11. Find all details of the prize won by PETER GRÜNBERG (Non-ASCII characters)
```sql
SELECT *
FROM nobel
WHERE winner LIKE '%Grünberg%'
```
12. Find all details of the prize won by EUGENE O'NEILL
```sql
SELECT *
FROM nobel
WHERE winner = 'EUGENE O''NEILL'
```
13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
Направление сортировки: ASC (по возрастанию) или DESC (по убыванию)
```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC
```
14. 
```sql
```

```sql
```

```sql
```