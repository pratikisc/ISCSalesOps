---
title: Transformation (Adding a new key column)
View Name: sfdc-w003v1-t003-teamnames__s
Status: Interim View
---

```sql


SELECT
    s.casenumber || '__S' || right(s.team, 1) AS id,
    s.casenumber,
    s.teamvalue
FROM
    "sfdc-w003v1-t002-teamnames" as s;
    
    
