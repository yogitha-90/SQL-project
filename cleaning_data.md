What issues will you address by cleaning the data?

create table allsessionsdistinct
as select distinct on (fullvisitorid)*
from allsessions
order by fullvisitorid;
drop table allsessions;
ALTER TABLE allsessionsdistinct
RENAME TO allsessions;

select cast (fullvisitorid as numeric)
from allsessions;

For country from not set to na
update allsessions
set country = case
when country = '(not set)' then 'n/a'
else country
end;

Updating city name as country where not indicated

update allsessions
set city = case
when city= 'not available in demo dataset' then country
else city
end;

Search for empty columns and dropping them
For example
select searchkeyword from allsessions
where searchkeyword != null;
 
Therefore drop search keyword column from all sessions table.
alter table allsessions
drop column searchkeyword;

Similarly

alter table allsessions
drop column itemrevenue;

alter table allsessions
drop column itemquantity;

With regards to analysis table:

Change visitstarttime:

select to_timestamp(cast(visitstarttime as integer))::time as convertedtime
from analytics
limit 10;


Updated the unit price by dividing the unit price/1000000
update analytics
set unitprice=unitprice/1000000;

Deleting the column userid
alter table analytics
drop column userid;

