---
view: '"commissions"."plan-rule-2021-001-t001-amer-psm-inet-launches"'
---

```
select
a.casenumber,
b.st_id_amer as amer_psm_sub_territory
from
salesforce_case as a
left join "commissions"."reference-sfdc account and ship to amer sub terr" as b
on a.accountid = b.id
where
a.type = 'Launch'
and a.firstin_partner_account__c is not null
```
