---
view: '"commissions"."plan-rule-2021-001-t001-amer-psm-inet-launches"'
---

```sql
select
a.casenumber,
b.st_id_amer as amer_psm_sub_territory
from
salesforce_case as a
-- !!! Relation with Account Ship To and Sub Territory Id is done in another view referenced here
left join "commissions"."reference-sfdc account and ship to amer sub terr" as b
on a.accountid = b.id
where
-- !!! Launches only
a.type = 'Launch'  
-- !!! Launches with partner accounts only
and a.firstin_partner_account__c is not null 
```
