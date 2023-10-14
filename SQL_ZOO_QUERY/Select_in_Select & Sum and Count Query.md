
## SELECT in SELECT 

1. List each country name where the population is larger than that of 'Russia'.
Table Column: world(name, continent, area, population, gdp)
```bash
Select name FROM worldvWHERE population > (SELECT population FROM world WHERE name='Russia');
```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```bash
Select name from world where continent = 'Europe' and gdp/population > (Select gdp/population from world where name = 'United Kingdom');
```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```bash
Select name, continent from world where continent in (Select continent from world where name in ('Argentina','Australia')) order by name ;
```
4. Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
```bash
Select name, population from world where population > (Select population from world where name = 'United Kingdom') and population < (Select population from world where name = 'Germany');
```
5. 
6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```bash
Select name from world where gdp > (Select max(gdp) from world where continent = 'Europe');
```
7. Find the largest country (by area) in each continent, show the continent, the name and the area:
```bash
SELECT continent, name, area FROM world x WHERE area >= ALL (SELECT area FROM world y WHERE y.continent=x.continent AND area >0);
```
8. List each continent and the name of the country that comes first alphabetically.
```bash
Select continent,name 
from world x
Where x.name <= ALL(select y.name from world y
                    where x.continent=y.continent)
ORDER BY continent;
```
9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```bash
Select name, continent, population from world x where continent not in (Select distinct continent from world  where population < = 25000000);
```

## SUM & COUNT 

1. Show the total population of the world.
```bash
Select Sum(Population) from world;
```
2. List all the continents - just once each.
```bash
Select distinct continent from world;
```
3. Give the total GDP of Africa
```bash
Select Sum(gdp) from world where continent = 'Africa';
```
4. How many countries have an area of at least 1000000.
```bash
Select count(name) from world where area > = 1000000
```
5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```bash
Select sum(population) from world where name in ('Estonia', 'Latvia', 'Lithuania');
```
6. For each continent show the continent and number of countries.
```bash
Select continent, count(name) from world group by continent;
```
7. For each continent show the continent and number of countries with populations of at least 10 million.
```bash
Select continent, count(name) from world where population >= 10000000 group by continent;
```
8. List the continents that have a total population of at least 100 million.
```bash
Select continent from world group by continent having sum(population)>=100000000;
```
