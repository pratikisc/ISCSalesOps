---
view: '"commissions"."sfdc-w004-t001-msaoverrides"'
---

```sql

select

a.casenumber,
nbvlocal,
distributor_commission__c,
spiff_commission__c,
contract_length__c,
b.__sub_territory_id AS msaownerdmterritory,
a.booked_date__c

from
"commissions"."sfdc-case-w0001v2-t0005-grouped-and-non-grouped-with-attributes-final" as a
left join "territory"."sheets_join_territory_join sfdc case dm name" as b ON a.msaownerid = b.__dm__c
where datediff(day,msastartdate::date,booked_date__c::date) < 366 and
booked_Date__c > '2021-01-01' and
lower(msastatus) like '%activ%'   and
b.__sub_territory_id - a.dm_sub_territory_id <> 0

```
