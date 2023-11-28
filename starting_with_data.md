Question 1: Find popular visitingtime (hours) for visitors?

SQL Queries:select extract (hours from to_timestamp(cast(visitstarttime as integer)))
as visithour,
count(*)as visitcount
from analytics
group by visithour
order by visithour;


Answer: ![AA5AA2D7-A67C-4D25-BB92-227043132CD8](https://github.com/yogitha-90/SQL-project/assets/145248979/7383b70c-897d-409d-b044-00e73babbb44)



Question 2: what is the most popular channel grouping?

SQL Queries:
select channelgrouping,
count(*)from allsessions
group by channelgrouping
order by count(*) desc;
![F0FDFEBD-EDF3-4BA0-9957-36A0827B52FA](https://github.com/yogitha-90/SQL-project/assets/145248979/02b51f11-a372-4c49-9924-f856a8bdc2f7)



Question 3: Which country generates max revenue?

SQL Queries:select country,sum(totaltransactionrevenue) as countryrevenue from allsessions
where totaltransactionrevenue is not null
group by country
order by countryrevenue desc;

Answer:![8D2CDB39-10FC-40C7-BDB7-9431D5BD74B9](https://github.com/yogitha-90/SQL-project/assets/145248979/008e9b1f-70b2-4588-9842-45919b025db7)

