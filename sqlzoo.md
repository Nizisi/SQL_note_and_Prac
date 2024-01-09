## find whether column string is substring of another column
	WHERE t1.FullName LIKE CONCAT('%', t2.Name, '%')
## Replace letter in a string value column
	REPLACE('vessel','e','a') -> 'vassal'
	
	### This replacement can even use to remove certain letter in a string
		SELECT name,
			   REPLACE(name, 'a','')
		  FROM bbc
