---
title: Transformation (Adding a new key column)
View Name: sfdc-w003-t003-bookingvalues__s
Status: Interim View
---

```sql


SELECT
    s.casenumber || '__S' || right(s.valueorder, 1) AS id,
    s.casenumber,
    s.bookingvalue
FROM
    "sfdc-w003-t002-bookingvalues" as s;
