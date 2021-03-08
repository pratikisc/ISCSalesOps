---
title: Joining on the new casenumber__S key
View Name: sfdc-w003-t004-unpivoted-key-values
Status: Interim View
---

```sql


SELECT
    t.id,
    t.casenumber,
    t.teamname,
    b.bookingvalue
FROM
    (
        "sfdc-w003-t003-teamnames__s" t
        LEFT JOIN "sfdc-w003-t003-bookingvalues__s" b ON ((t.id = b.id))
    )
WHERE
    (
        t.teamname IS NOT NULL
        AND 
        b.bookingvalue <> 0
    )
ORDER BY
    t.id;
