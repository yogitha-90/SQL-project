What issues will you address by cleaning the data?

create table allsessionsdistinct
as select distinct on (fullvisitorid)*
from allsessions
order by fullvisitorid;
drop table allsessions;
ALTER TABLE allsessionsdistinct
RENAME TO allsessions;





Queries:
Below, provide the SQL queries you used to clean your data.
