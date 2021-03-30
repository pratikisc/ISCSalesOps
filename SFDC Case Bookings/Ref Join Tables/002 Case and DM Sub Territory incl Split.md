---
View: '"commissions"."reference-sfdc-case-dm-subterritory-incl-splits"'
'!!! CAUTION': This table has non unique rows as 'casenumber'. Be careful when using as a foreign key, it will add duplicate rows on casenumber match
---

```sql
select
a.casenumber,
c.id as split_id,
coalesce(c.__sub_territory_id, b.__sub_territory_id) as dm_sub_territory_incl_split,
coalesce(c.allocation,1) as allocation
from
salesforce_case a
left join "territory"."sheets_join_territory_join sfdc case dm name" b ON a.dm__c = b.__dm__c
left join "commissions"."sfdc-w003v3-t005-split-ref-table" c ON a.casenumber = c.casenumber
where
a.finance_sub_status__c = 'Booked'

```
