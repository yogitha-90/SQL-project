Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:



Answer:![C7FD0065-8202-45BA-8B2C-79D9DB0217C2](https://github.com/yogitha-90/SQL-project/assets/145248979/1b17fde8-8fd2-4a21-be71-475bae98ba5a)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:select cast(avg(a.totalproductssold) as int)as avgproductssold, a.city,a.country from 
(select sum (productquantity)as totalproductssold,
city,country from allsessions
group by city,country) a group by a.city, a.country
having avg(a.totalproductssold)<> 0;











**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







