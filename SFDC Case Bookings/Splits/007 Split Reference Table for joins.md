---
title: get allocations for left joins on main table
View Name: '"commissions"."sfdc-w003v3-t005-split-ref-table"'
Note: Split Sub Territory override for cases where split > 100% included here
PK: id
---

```sql


select
a.id,
a.casenumber,
a.teamvalue,
coalesce(d.__sub_territory_id, c.__sub_territory_id) as __sub_territory_id,
a.bookingvalue,
b.net_bookings_value__c,
a.bookingvalue / b.net_bookings_value__c as allocation,
'Split Case' as calculationflag
from
"commissions"."sfdc-w003v2-t004-unpivoted-key-values" as a
left join salesforce_case as b on a.casenumber = b.casenumber
left join "territory"."sheets_join_territory_join sfdc case split values" c on a.teamvalue = c.__teamvalue
left join "override"."sheets_join sfdc case overrides_join_sfdc split case sub territory overrides" d on a.id = d.__casenumber__sx
where
b.net_bookings_value__c <> 0
and teamvalue not like 'GAM%'
order by a.id
```
