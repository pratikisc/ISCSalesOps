---
title: get allocations for left joins on main table
View Name: '"commissions"."sfdc-w003v3-t005-split-ref-table"'
Status: Final reference join table
---

```sql


select
a.id,
a.casenumber,
a.teamvalue,
c.__sub_territory_id,
a.bookingvalue,
b.net_bookings_value__c,
a.bookingvalue / b.net_bookings_value__c as allocation,
'Split Case' as calculationflag
from
"commissions"."sfdc-w003v2-t004-unpivoted-key-values" as a
left join salesforce_case as b on a.casenumber = b.casenumber
left join "territory"."sheets_join_territory_join sfdc case split values" c on a.teamvalue = c.__teamvalue
where
b.net_bookings_value__c <> 0
and teamvalue not like 'GAM%'
order by a.id
