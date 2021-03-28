---
title: get allocations for left joins on main table
View Name: '"commissions"."sfdc-w003v2-t005-split-ref-table"'
Status: Final reference join table
---

```sql


select
a.id,
a.casenumber,
a.teamvalue,
a.bookingvalue,
b.net_bookings_value__c,
a.bookingvalue / b.net_bookings_value__c as allocation
from
"commissions"."sfdc-w003v2-t004-unpivoted-key-values" as a
left join salesforce_case as b
on a.casenumber = b.casenumber
where
b.net_bookings_value__c <> 0
and teamvalue not like 'GAM%'
order by a.id