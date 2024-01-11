# SELECT from World
## find whether column string is substring of another column
	WHERE t1.FullName LIKE CONCAT('%', t2.Name, '%')
 <br/>The function concat is short for concatenate - you can use it to combine two or more strings.<br/>
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
