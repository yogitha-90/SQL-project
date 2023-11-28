What are your risk areas? Identify and describe them.
The data provided was not accurate,it had several duplictes and null rows
to get accurate results Duplicates much be removed, so that the unique id's can be identified.
Data types should be matched perfectly to prevent any errors.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
removing duplicates from allsessions table:
sql 
select distinct on (fullvisitorid)*
from allsessions

 What is the average number of products ordered from visitors in each city and country?**
to check the result rendered from this query is true then when calculated for columbia, the value should return 1
SQL 



