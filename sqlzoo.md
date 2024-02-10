## find whether column string is substring of another column
	WHERE t1.FullName LIKE CONCAT('%', t2.Name, '%')
 <br/>The function concat is short for concatenate - you can use it to combine two or more strings.<br/>
 You can also use it to add symbol.
## Replace letter in a string value column
	REPLACE('vessel','e','a') -> 'vassal'
	
	### This replacement can even use to remove certain letter in a string
		SELECT name,
			   REPLACE(name, 'a','')
		  FROM bbc
## To round up a column
	ROUND(7253.86, 0)    ->  7254
	ROUND(7253.86, 1)    ->  7253.9
 	ROUND(7253.86,-3)    ->  7000
## Find specific letter from the start of the string
LEFT(s,n) allows you to extract n characters from the start of the string s.

   LEFT('Hello world', 4) -> 'Hell'     
# SELECT from Nobel Tutorial
## When single quote inside quote string already, use two quote
	SELECT * FROM nobel
	WHERE winner = "EUGENE O'NEILL"
 ## In clause can be used as value 1 or 0  (boolean)
 The expression subject IN ('chemistry','physics') can be used as a value - it will be 0 or 1.
 ## We can use the word ALL to allow >= or > or < or <=to act over a list. 
```
SELECT name
FROM world
WHERE population >= ALL(SELECT population
                           FROM world
                          WHERE population>0)
```
You need the condition population>0 in the sub-query as some countries have null for population.
## We can refer to values in the outer SELECT within the inner SELECT.
## correlated subqueries
	A correlated subquery works like a nested loop: the subquery only has access to rows related to a single record at a time in the outer query.
	One way to interpret the line in the WHERE clause that references the two table is “… where the correlated values are the same”.
 	A correlated subquery is used some action must be taken on each row in your query that depends on one or more values from that row.

  ### example
  ```
  SELECT name, continent, population 
	FROM world w
	WHERE NOT EXISTS (                  -- there are no countries
	   SELECT *
	   FROM world nx
	   WHERE nx.continent = w.continent -- on the same continent
	   AND nx.population > 25000000     -- with more than 25M population 
	   );
```
## GROUP BY and AGGREGATE FUNC
	Group By X means put all those with the same value for X in the one group.

	Group By X, Y means put all those with the same values for both X and Y in the one group.
 ### EXAMPLE:
  ```
 Show the year and subject where 3 prizes were given. Show only years 2000 onwards.
	SELECT yr, subject
	FROM nobel
	WHERE yr >= 2000
	GROUP BY yr, subject
	HAVING COUNT(winner) = 3
 ```
## Ways to return different values under different conditions.
USE CASE WHEN!
```
  CASE WHEN condition1 THEN value1 
       WHEN condition2 THEN value2  
       ELSE def_value 
  END
```
```
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT JOIN goal ON matchid = id
GROUP BY matchid,team1,team2
ORDER BY mdate,matchid,team1,team2
```
CASE allows you to return different values under different conditions. If there no conditions match (and there is not ELSE) then NULL is returned. <br/>
GROUP BY refer to above GROUP BY and AGGREGATE FUNC section.
## COALESCE- way to find  first value that is not null, or return default value if null
```
  COALESCE(x,y,z) = x if x is not NULL
  COALESCE(x,y,z) = y if x is NULL and y is not NULL

SELECT name, COALESCE(mobile,'07986 444 2266')  FROM teacher ##this return '07986 444 2266' if mobile is null
```
## Rank Function
Returns the rank of each row within the partition of a result set. The rank of a row is one plus the number of ranks that come before the row in question.
```
This query rank the row base on votes desc, so the row with most vote is rank 1 .etc
SELECT party, votes,
       RANK() OVER (ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000024 ' AND yr = 2017
ORDER BY party

```
PARTITION BY clause divide the table into different blocks, for example below, base on the same yr. 
```
SELECT yr,party, votes,
      RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000021'
ORDER BY party,yr
```
## LAG function
The LAG function is used to show data from the preceding row or the table. <br/>
https://sqlzoo.net/wiki/Window_LAG (detailed graph)
```
SELECT name, DAY(whn), confirmed,
   LAG(whn, 1) OVER (PARTITION BY name ORDER BY whn)   ## this lag function, starting from second row, show the whn from the previous row
 FROM covid
WHERE name = 'Italy'
AND MONTH(whn) = 3 AND YEAR(whn) = 2020
ORDER BY whn

```
We can use lag function to calculate difference between rows like example below:
```
SELECT name, DAY(whn),
  (confirmed - LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn)) AS day_count
 FROM covid
WHERE name = 'Italy'
AND MONTH(whn) = 3 AND YEAR(whn) = 2020
ORDER BY whn

```
## Self Join
https://sqlzoo.net/wiki/Using_a_self_join <br/>
FOR EXAMPLE:
```
We might join the route table with itself on the stop field. The result is a list of all pairs of services which share a stop.

SELECT * FROM route R1, route R2

  WHERE R1.stop=R2.stop;
```
## Cross Join
To get every possible combination of those two tables.
