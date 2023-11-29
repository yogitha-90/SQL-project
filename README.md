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
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
