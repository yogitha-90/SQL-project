##Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


//SQL Queries:

```SQL
select country, city, totalrevenue
from (select country,city, max(totaltransactionrevenue)as totalrevenue,
	 Rank() over (partition by country order by max(totaltransactionrevenue)desc)as Rank
	 from allsessions where totaltransactionrevenue is not null group by country,city)as sub
	 where rank=1;
```



![EDA49EA6-0BA7-425D-8B0D-63118F6A987D](https://github.com/yogitha-90/SQL-project/assets/145248979/20c6c98d-064a-4141-b29f-e76641a56efb)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


//SQL Queries:

```SQL
select cast(avg(a.totalproductssold) as int)as avgproductssold, a.city,a.country from 
(select sum (productquantity)as totalproductssold,
city,country from allsessions
group by city,country) a group by a.city, a.country
having avg(a.totalproductssold)<> 0;
```

![6C036AC1-30F5-4713-A9FE-5C2324CFEF09](https://github.com/yogitha-90/SQL-project/assets/145248979/927cafd8-39a4-4fed-96ad-897770294c3c)


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


//SQL Queries:

```SQL
Create temp table overall_cate as
select country,city, p.name as productname,v2productcategory, sum(orderedquantity) as totalproducts
from allsessions s join products p on s.productsku =p.sku
group by country, city, p.name, v2productcategory
having sum(orderedquantity)<>0
order by country, city, totalproducts desc;
```

Ans: ![99DA39CC-2FE0-4C50-805F-92C1DDDF2CE5](https://github.com/yogitha-90/SQL-project/assets/145248979/0f66069a-f2a6-49db-8543-66a6f05295b2)

//finding pattern

//Most popular:

```SQL
select country,city,v2productcategory, max(totalproducts) as order,
'most popular' as type
from overallcate
group by country,city, v2productcategory
order by max(totalproducts)desc
limit 3;
```
![4A661250-7C9C-4FFE-B590-EB8D1EAC2E0A](https://github.com/yogitha-90/SQL-project/assets/145248979/948e8e8b-2357-4cf9-9938-653fb1c9df4a)

//least popular:

```SQL
select country,city,v2productcategory, min(totalproducts) as order,
'least popular' as type
from overallcate
group by country,city, v2productcategory
order by min(totalproducts)desc
limit 3;
```
![EF97CFCD-7E96-4542-AC58-FA1DD1C578E1](https://github.com/yogitha-90/SQL-project/assets/145248979/80e8e0c3-77bf-44aa-9de8-901334493ad7)


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


//SQL Queries:
//creating a cte table

```SQL

with salebycountry as (select country, city, p.name as productname,
sum(orderedquantity)as totalproducts,
rank()over (partition by country, city order by sum(orderedquantity)desc)as rank
from allsessions s
join products p on s.productsku=p.sku
group by country,city,p.name
order by country,city);
```
//selecting highest selling product with in each country and city.

```SQL
select country,city,productname
from salebycountry
where rank=1;
```

![2D8776BF-1160-4689-BF1C-A17D5AA64687](https://github.com/yogitha-90/SQL-project/assets/145248979/a07da32f-ac77-4d49-9e66-b42d391c9c34)


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

//SQL Queries:

```SQL
select country,city,
sum(totaltransactionrevenue)as revenue
from allsessions
group by country,city
having sum(totaltransactionrevenue)<>0;
```
//only 81 records of 15000 records have the data, these are the results obtained by them

![F486E08B-C501-4272-B0D0-E161E94EDF9C](https://github.com/yogitha-90/SQL-project/assets/145248979/16320bb0-4664-4c4e-a521-90f96406c6dd)
