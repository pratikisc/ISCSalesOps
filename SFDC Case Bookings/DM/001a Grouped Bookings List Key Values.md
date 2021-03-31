---
View: '"commissions"."sfdc-case-w0002-dm-t0001ref-grouped-cases"'
---
```sql

SELECT
id_h
,listagg ( casenumber, ', ') AS case_list
,listagg ( net_bookings_value__c::int, ', ') nbvlocal_list
FROM   "commissions"."sfdc-case-w0002-dm-t0000-base-table"
WHERE id_h is not null
GROUP  BY 1

```
