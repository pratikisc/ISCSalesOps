---
View: '"commissions"."plan-rule-2021-002-t001-isc-reps-safer-launches-only"'
PK: casenumber (This view is not yet scripted)
Clause: DMs, GAMs and KAMs do not get credit for SAFER cases except launch. This removes credit for all but launch cases.
Override: If a DM, GAM or KAM needs to get credit, then an overide will need to be inserted in Captivate IQ
---
```sql
select
casenumber,
null::character varying (200) as sub_territory_null
from
salesforce_case
where
finance_sub_status__c = 'Booked'
and type not in ('Launch', 'Amendment')
and inet_type__c = 'SAFER'

```
