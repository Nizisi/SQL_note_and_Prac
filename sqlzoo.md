## find whether column string is substring of another column
	WHERE t1.FullName LIKE CONCAT('%', t2.Name, '%')
 <br/>The function concat is short for concatenate - you can use it to combine two or more strings.<br/>
## Replace letter in a string value column
	REPLACE('vessel','e','a') -> 'vassal'
	
	### This replacement can even use to remove certain letter in a string
		SELECT name,
			   REPLACE(name, 'a','')
		  FROM bbc
