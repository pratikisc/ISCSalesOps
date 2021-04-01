---
View: '"commissions"."sfdc-case-w0001v2-t0001ref-grouped-cases"'
"'
PK: id_h
---
```sql

SELECT
id_h
,listagg ( casenumber, '; ') AS case_list
,listagg ( net_bookings_value__c::int, '; ') nbvlocal_list
FROM   "commissions"."sfdc-case-w0001v2-t0000-a1-base-table"
WHERE id_h is not null
GROUP  BY id_h

```
