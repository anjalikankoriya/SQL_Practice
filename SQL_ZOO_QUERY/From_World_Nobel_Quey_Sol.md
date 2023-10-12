
## SELECT from WORLD

1. Observe the result of running this SQL command to show the name, continent and population of all countries.
```bash
  Select name, continent, population FROM world;
```
2. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
```bash
Select name FROM world WHERE population >= 200000000;
```
3. Give the name and the per capita GDP for those countries with a population of at least 200 million.
HELP: Per_Capita_Gdp = GDP/population
```bash 
Select name, (GDP/Population) as per_capita_Gdp from world where population >= 200000000;
```
4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```bash
Select name, (population/1000000) as Population_million from world where continent = 'South America';
```
5. Show the name and population for France, Germany, Italy.
```bash
select name, population from world where name in ('France','Germany','Italy');
```
6. Show the countries which have a name that includes the word 'United'.
```bash
Select name from world where name like '%United%';
```
7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name, population and area.
```bash
select name, population, area from world where area > 3000000 or population > 250000000;
```
8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.

- Australia has a big area but a small population, it should be included.
- Indonesia has a big population but a small area, it should be included.
- China has a big population and big area, it should be excluded.
- United Kingdom has a small population and a small area, it should be excluded.
```bash
select name, population, area from world where (area > 3000000 and not population > 250000000) or (population > 250000000 and not area >3000000);
```
9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.

For Americas show population in millions and GDP in billions both to 2 decimal places.

NOTE: For some version of SQL the division of an integer by an integer will be an integer. One way to prevent this is to divide by a floating point number such as 1000000.0.
```bash
select name, Round(population/1000000.0,2) as Population_M , Round(GDP/1000000000.0,2) as GDP_B from world where continent = 'South America';
```
10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.

Show per-capita GDP for the trillion dollar countries to the nearest $1000.
```bash
select name, Round(GDP/Population, -3) as per_capita_GDP from world where GDP >= 1000000000000;
```
11. The capital of Sweden is Stockholm. Both words start with the letter 'S'. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
- You can use the function LEFT to isolate the first character.
- You can use <> as the NOT EQUALS operator.

HINT: The LEFT function is used to extract the first letter of both the country name and the capital.
```bash
Select name, capital FROM world where LEFT(name,1) = LEFT(Capital,1) and name <> capital;
```
12. Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name.

Find the country that has all the vowels and no spaces in its name.
- You can use the phrase name NOT LIKE '%a%' to exclude characters from your results.
- The query shown misses countries like Bahamas and Belarus because they contain at least one 'a'.
```bash 
Select name FROM world WHERE name NOT LIKE '% %'
  AND name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%';
  ```

## SELECT from NOBEL

1. Change the query shown so that it displays Nobel prizes for 1950.
```bash
Select yr, subject, winner FROM nobel WHERE yr = 1950;
```
2. Show who won the 1962 prize for literature.
```bash
Select winner from nobel where yr = 1962 and subject = 'Literature';
```
3. Show the year and subject that won 'Albert Einstein' his prize.
```bash
Select yr, subject from nobel where winner = 'Albert Einstein';
```
4. Give the name of the 'peace' winners since the year 2000, including 2000.
```bash
Select winner as winner_peace from nobel where subject = 'Peace' and  yr >= 2000;
```
5. Show all details (yr, subject, winner) of the literature prize winners for 1980 to 1989 inclusive.
```bash
Select yr, subject, winner from nobel where subject = 'Literature' and yr between '1980' and '1989';
```
6. Show all details of the presidential winners:
- Theodore Roosevelt
- Thomas Woodrow Wilson
- Jimmy Carter
- Barack Obama
```bash 
Select * from nobel where subject = 'Peace' and winner in ('Theodore Roosevelt', 'Woodrow Wilson','Jimmy Carter',
'Barack Obama');
```
7. Show the winners with first name John.
```bash
Select winner from nobel where winner like 'John%';
```
8. Show the year, subject, and name of physics winners for 1980 together with the chemistry winners for 1984.
```bash
Select yr, subject, winner from nobel where (subject = 'Physics' and yr = 1980) or (subject = 'Chemistry' and yr = 1984);
```
9. Show the year, subject, and name of winners for 1980 excluding chemistry and medicine.
```bash
Select yr, subject, winner from nobel where subject not in ('Chemistry', 'Medicine') and yr = 1980;
```
10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004).
```bash
Select yr, subject, winner from nobel where (subject = 'Medicine' and yr < 1910) or (subject = 'Literature' and yr >= 2004);
```
11. Find all details of the prize won by PETER GRÜNBERG.
```bash
Select * from nobel where winner = 'PETER GRÜNBERG'
```
12. Find all details of the prize won by EUGENE O'NEILL
Escaping single quotes:
- You can't put a single quote in a quote string directly. - You can use two single quotes within a quoted string.
```bash
Select * from nobel where winner = 'EUGENE O''NEILL';
```
13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```bash
Select winner, yr, subject from nobel where winner like 'Sir%' order by yr desc, winner;
```
14. Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last.
```bash
select winner, subject from nobel where yr = 1984
order by
    case
        when subject in ('Physics', 'Chemistry') then 1
        else 0
    end, subject, winner;
    ```


