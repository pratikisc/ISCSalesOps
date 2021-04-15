---
view: '"commissions"."plan-rule-2021-004-t001-amer-tk-total-safety"'
---

```sql
select
a.casenumber AS caseid,
a.casenumber :: text || ' - PSM Total Safety Amer' :: text AS casenumber,
c.name,
7011::bigint as tk_total_safety_sub_terr_id
from
salesforce_case as a

-- !!! Relation with Account Ship To and Sub Territory Id is done in another view referenced here
left join "commissions"."reference-sfdc account and ship to amer sub terr" as b on a.accountid = b.id
left join salesforce_account as c on a.firstin_partner_account__c = c.id

where

-- !!! Launches only
a.type = 'Launch'  

-- !!! Launches with partner accounts only
and a.firstin_partner_account__c is not null

and lower(c.name) like '%total safety%'

-- !!! Americas Only
and b.st_id_amer is not null

```
