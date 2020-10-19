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
## Additional material:
1. [Using GROUP BY and HAVING](#group-by-and-having)

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

## NESTED SELECT
1.1 List each country name where the population is larger than that of 'Russia'.
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```
1.2 Another variant
```sql
SELECT name, population
FROM world
WHERE name = 'Russia' //population = 146745098
```
```sql
SELECT name
FROM world
WHERE population > 146745098
```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'. Per Capita GDP.
```sql
SELECT name
FROM world
WHERE continent = 'Europe' 
  AND gdp/population > (SELECT gdp/population 
                        FROM world WHERE name = 'United Kingdom')
```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent
FROM world
WHERE continent IN ('South America', 'Oceania')
ORDER BY name
```
4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Canada') 
AND population < (SELECT population FROM world WHERE name = 'Poland')
```
5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.  
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.  
The format should be Name, Percentage for example:  
________________________  
|name     | percentage |  
------------------------  
| Albania |     3%     |  
| Andorra |     0%     |  
|....  
```sql
SELECT name, 
       CONCAT(ROUND(100*population/(SELECT population 
                                    FROM world 
                                    WHERE name='Germany'))
       , '%') as percentage
FROM world
WHERE continent = 'Europe'
```
6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
SELECT name
FROM world
WHERE gdp > ALL (SELECT gdp FROM world WHERE gdp > 0 AND continent = 'Europe')
```
7. Find the largest country (by area) in each continent, show the continent, the name and the area:
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area >0)
```
8. First country of each continent (alphabetically). List each continent and the name of the country that comes first alphabetically.
```sql
SELECT continent, name
FROM world as x
WHERE name = (SELECT name FROM world as y WHERE x.continent = y.continent LIMIT 1)
```
9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population
FROM world as x
WHERE continent != ALL(SELECT continent FROM world WHERE population > 25000000)
```
10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
```sql
- NONE
```
## SUM and COUNT
0.1. The total population and GDP of Europe.
```sql
SELECT SUM(population), SUM(gdp)
  FROM bbc
  WHERE region = 'Europe'
```
0.2. What are the regions?
```sql
SELECT DISTINCT region FROM bbc
```
0.3. Show the name and population for each country with a population of more than 100000000. Show countries in descending order of population.
```sql
SELECT name, population
  FROM bbc
  WHERE population > 100000000
  ORDER BY population DESC
```
1. Show the total population of the world.
```sql
SELECT SUM(population)
FROM world
```
2. List all the continents - just once each.
```sql
SELECT DISTINCT continent
FROM world
```
3. Give the total GDP of Africa
```sql
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa'
```
4. How many countries have an area of at least 1000000
```sql
SELECT COUNT(name)
FROM world
WHERE area > 1000000
```
5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```sql
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```
6. For each continent show the continent and number of countries.
```sql
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
```
7. For each continent show the continent and number of countries with populations of at least 10 million.
```sql
SELECT continent, COUNT(name)
FROM world
WHERE population > 10000000
GROUP BY continent
```
8. List the continents that have a total population of at least 100 million.
```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```
## JOIN
<img src="img/6-1 Stage - Table Games.png"/>
<img src="img/6-2 Stage - Table goal.png"/>
<img src="img/6-3 Stage - Table eteam.png"/>
1.1 Shows the goal scored by a player with the last name 'Bender'.  
The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime
```sql
SELECT * FROM goal 
  WHERE player LIKE '%Bender'
```
1.2 Show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
SELECT matchid, player
FROM goal
WHERE teamid = 'GER'
```
2. Show id, stadium, team1, team2 for just game 1012
```sql
SELECT id,stadium,team1,team2
FROM game
WHERE id = '1012'
```
3. You can combine the two steps into a single query with a JOIN.
```sql
SELECT *
  FROM game JOIN goal ON (id=matchid)
```
The FROM clause says to merge data from the goal table with that from the game table.  
The ON says how to figure out which rows in game go with which rows in goal - the matchid from goal must match id from game.  
(If we wanted to be more clear/specific we could say ON (game.id=goal.matchid)
The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.
Modify it to show the player, teamid, stadium and mdate for every German goal.
```sql
SELECT player, teamid, stadium, mdate
FROM goal JOIN game ON (goal.matchid=game.id)
WHERE teamid='GER'
```
4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
SELECT team1, team2, player
FROM game JOIN goal ON (id=matchid)
WHERE player LIKE 'Mario%'
```

```sql
```

```sql
```

```sql
```
## Using GROUP BY and HAVING
1. For each continent show the number of countries:
```sql
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
```
2. For each continent show the total population:
```sql
SELECT continent, SUM(population)
FROM world
GROUP BY continent
```
3. WHERE and GROUP BY. The WHERE filter takes place before the aggregating function.  
For each relevant continent show the number of countries that has a population of at least 200000000.
```sql
SELECT continent, COUNT(name)
FROM world
WHERE population > 200000000
GROUP BY continent
```
4. GROUP BY and HAVING. The HAVING clause is tested after the GROUP BY.  
You can test the aggregated values with a HAVING clause.  
Show the total population of those continents with a total population of at least half a billion.
```sql
SELECT continent, SUM(population)
FROM world
GROUP BY continent
HAVING SUM(population) > 500000000
```

```sql
```

```sql
```

```sql
```

```sql
```

```sql
```

```sql
```