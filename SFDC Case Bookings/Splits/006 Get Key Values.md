---
title: Joining on the new casenumber__S key
View Name: '"commissions"."sfdc-w003v2-t004-unpivoted-key-values"'
Status: Interim View
---

```sql


SELECT
    t.id,
    t.casenumber,
    t.teamvalue,
    b.bookingvalue
FROM
    "commissions"."sfdc-w003v2-t003-teamnames__s" t
    LEFT JOIN "commissions"."sfdc-w003v2-t003-bookingvalues__s" b ON t.id = b.id
WHERE
    t.teamvalue IS NOT NULL
    AND 
    b.bookingvalue <> 0
ORDER BY
    t.id;
