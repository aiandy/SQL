# SQL

#List each country name where the population is larger than that of 'Russia'.
select name from world
where population>(
select population from world
where name='Russia')

#Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
select name from world
where continent='Europe' and GDP/population>(
select gdp/population from world
where name='United Kingdom'
)

#List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
select name,continent from world
where continent=(select continent from world
where name like '%Argentina%') or continent=(select continent from world where name like '%Australia%')
order by name

#Which country has a population that is more than Canada but less than Poland? Show the name and the population.
select name,population from world
where population>(select population from world where name='Canada'
) and population<(select population from world where name='Poland')

#Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
select name,concat(round(population/(select population from world where name='Germany')*100,0),'%') from world
where continent='Europe'

#Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
select name from world
where GDP>=all(
select gdp from world
where continent='Europe' and gdp>0
) and continent<>'Europe'

#Find the largest country (by area) in each continent, show the continent, the name and the area:
SELECT continent,name,area 
    FROM world x
WHERE area>= ALL(
     SELECT area FROM world y
     WHERE x.continent=y.continent AND area>0)
     
#List each continent and the name of the country that comes first alphabetically.
SELECT continent,min(name) FROM world
GROUP BY continent
ORDER BY continent
