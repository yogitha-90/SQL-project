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


