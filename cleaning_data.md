##What issues will you address by cleaning the data?

##adressing allsessions table

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

```SQL
alter table analytics
drop column userid;
```
