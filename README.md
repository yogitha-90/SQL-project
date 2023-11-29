# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

(1) Importing ecommerce data from files to postgreSQL tables.
(2) Cleaning and transforming data for analytics.
(3) Study patterns and find relationships across different tables
(4) Running queries to get business insights.
(5) Implementing the process of quality analysis to evince business insights. 


## Process

## Step 1:

//Syntax for creating and importing data:

//For example:
//Table for salesbysku:

```SQL
Create Table salesbysku
(
 productsku    character varying,
 totalordered  integer,
 Primary key   (productsku)
);
```

//Importing data using copy statement

```SQL
Copy salesbysku(productsku, totalordered)
From '/Users/yogithakandhi/Downloads/all_sessions .csv'
Delimiter ','
Csv header;
```

##Step 2: Data cleaning

##Adressing allsessions table

//removing duplicates to achieve accurate results

```SQL
create table allsessionsdistinct
as select distinct on (fullvisitorid)*
from allsessions
order by fullvisitorid;
drop table allsessions;
ALTER TABLE allsessionsdistinct
RENAME TO allsessions;
```

//changing datatypes as required

```SQL
select cast (fullvisitorid as numeric)
from allsessions;
```

//Updating country name for country from not set to na


```SQL
update allsessions
set country = case
when country = '(not set)' then 'n/a'
else country
end;
```
//Updating city name as country where not indicated

```SQL
update allsessions
set city = case
when city= 'not available in demo dataset' then country
else city
end;
```
//Search for empty columns and dropping them

//For example
```SQL
select searchkeyword from allsessions
where searchkeyword != null;
 ```
//Therefore drop search keyword column from all sessions table.

```SQL
alter table allsessions
drop column searchkeyword;
```
//Similarly

```SQL
alter table allsessions
drop column itemrevenue;
```
```SQL
alter table allsessions
drop column itemquantity;
```
##With regards to analysis table:

//Change visitstarttime:

```SQL
select to_timestamp(cast(visitstarttime as integer))::time as convertedtime
from analytics
limit 10;
```

//Updated the unit price by dividing the unit price/1000000

```SQL
update analytics
set unitprice=unitprice/1000000;
```
//Deleting the column userid



## Results
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


![F486E08B-C501-4272-B0D0-E161E94EDF9C](https://github.com/yogitha-90/SQL-project/assets/145248979/16320bb0-4664-4c4e-a521-90f96406c6dd)


## Challenges 
// Cleaning the data was one of the biggest challenge

//Understanding the data and changing the datatypes as required

//information was missing in number of cells.. for example to calculate the revenue we had very few rows 15000 recodrs, thus I had to QA for every code to check if the answer is correct.

//for example: 

//to count the total number of rows with info for totaltransactioncolumn for table allsessions

```SQL
select count(totaltransactionrevenue)
from allsessions
where totaltransactionrevenue <> 0;
```

## Future Goals
// I would investigate furthe and invest additional time to analyse the data and create several business use cases.
