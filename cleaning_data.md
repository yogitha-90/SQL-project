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
For country from not se to na
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




Queries:
Below, provide the SQL queries you used to clean your data.
