---
View: '"commissions"."sfdc-case-w0001v2-t0001ref-grouped-cases"'
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

```sql
WITH fact_table AS (
    SELECT
    id_h
    ,listagg ( casenumber, '; ') AS case_list
    ,listagg ( net_bookings_value__c::int, '; ') nbvlocal_list
FROM   "commissions"."sfdc-case-w0001v2-t0000-a1-base-table"
WHERE id_h is not null
GROUP  BY id_h
),
dim_table AS (
    SELECT
    a.id_h   
    FROM   "commissions"."sfdc-case-w0001v2-t0000-a1-base-table" a
    WHERE a.id_h is not null
    GROUP  BY a.id_h
    HAVING count (distinct a.casenumber) > 1
)
select
a.id_h
, b.case_list
, b.nbvlocal_list
from dim_table a
inner join fact_table b ON a.id_h = b.id_h
```
