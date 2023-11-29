What are your risk areas? Identify and describe them.

Risk areas:

The data provided was not accurate,it had several duplictes and null rows

To get accurate results Duplicates much be removed, so that the unique id's can be identified.

Data types should be matched perfectly to prevent any errors.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

Removing duplicates from allsessions table to achieve accurate results

```sql 
select distinct on (fullvisitorid)*
from allsessions
```

checking if the query results are correct:

for example :

for the question, Which country generates max revenue?

```SQL
select country,sum(totaltransactionrevenue) as countryrevenue from allsessions
where totaltransactionrevenue is not null
group by country
order by countryrevenue desc;
```

Answer:![8D2CDB39-10FC-40C7-BDB7-9431D5BD74B9](https://github.com/yogitha-90/SQL-project/assets/145248979/008e9b1f-70b2-4588-9842-45919b025db7)

which means there should not be country which generates more that 11531530000, then the below sql statement should lead the result null.

```sql


select country,sum(totaltransactionrevenue) as countryrevenue from allsessions
where totaltransactionrevenue is not null
group by country, totaltransactionrevenue
having totaltransactionrevenue>11531530000;
```

![C65E8B79-400B-4826-884F-53BB5334936F](https://github.com/yogitha-90/SQL-project/assets/145248979/579d95fd-ea47-496a-ad27-d73fbdd7ddd7)


Thus proving above code to be accurate.


In allsessions table,

Products quantity should be greater than 0, for example:

```Sql
select productquantity 
from allsessions
where productquantity < 0;
```
which results in
![F410D4B6-AD7B-4F34-A7A7-58CBEC7CD5AD](https://github.com/yogitha-90/SQL-project/assets/145248979/8dd8c0d7-fb80-4185-b2da-2d5fe06f1f61)

therefore, it's accurate..

Smilarly checked the data for ordered quantity, stocklevel etc from produts table, unitprice from analytics table it rendered null result.





