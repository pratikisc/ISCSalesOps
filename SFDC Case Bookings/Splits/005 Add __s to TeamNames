---
title: Transformation (Adding a new key column)
View Name: sfdc-w003-t001-splittable
Status: Interim View
---

```sql


SELECT
    s.casenumber || '__S' || right(s.teamorder, 1) AS id,
    s.casenumber,
    s.teamname
FROM
    "sfdc-w003-t002-teamnamess" as s;
    
    
