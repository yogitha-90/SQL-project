Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
select city,country, max(totaltransactionrevenue) as totalrevenue
from allsessions
where totaltransactionrevenue is not null
group by city, country
order by max(totaltransactionrevenue) desc;

![image](https://github.com/yogitha-90/SQL-project/assets/145248979/b8b26cd6-63ff-470b-bf4a-eadf1d8acb3f)




Answer:![C7FD0065-8202-45BA-8B2C-79D9DB0217C2](https://github.com/yogitha-90/SQL-project/assets/145248979/1b17fde8-8fd2-4a21-be71-475bae98ba5a)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:select cast(avg(a.totalproductssold) as int)as avgproductssold, a.city,a.country from 
(select sum (productquantity)as totalproductssold,
city,country from allsessions
group by city,country) a group by a.city, a.country
having avg(a.totalproductssold)<> 0;











**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
Create temp table overall_cate as
select country,city, p.name as productname,v2productcategory, sum(orderedquantity) as totalproducts
from allsessions s join products p on s.productsku =p.sku
group by country, city, p.name, v2productcategory
having sum(orderedquantity)<>0
order by country, city, totalproducts desc
Ans: ![99DA39CC-2FE0-4C50-805F-92C1DDDF2CE5](https://github.com/yogitha-90/SQL-project/assets/145248979/0f66069a-f2a6-49db-8543-66a6f05295b2)

finding pattern
Most popular:
select country,city,v2productcategory, max(totalproducts) as order,
'most popular' as type
from overallcate
group by country,city, v2productcategory
order by max(totalproducts)desc
limit 3;
![4A661250-7C9C-4FFE-B590-EB8D1EAC2E0A](https://github.com/yogitha-90/SQL-project/assets/145248979/948e8e8b-2357-4cf9-9938-653fb1c9df4a)

least popular:
least popular:
select country,city,v2productcategory, min(totalproducts) as order,
'least popular' as type
from overallcate
group by country,city, v2productcategory
order by min(totalproducts)desc
limit 3;
![EF97CFCD-7E96-4542-AC58-FA1DD1C578E1](https://github.com/yogitha-90/SQL-project/assets/145248979/80e8e0c3-77bf-44aa-9de8-901334493ad7)










**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







