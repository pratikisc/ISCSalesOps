---
title: Transformation (Adding a new key column)
View Name: `"commissions"."sfdc-w003v2-t003-teamnames__s"`
Status: Interim View
---

```


SELECT
    s.casenumber || '__S' || right(s.team, 1) AS id,
    s.casenumber,
    s.teamvalue
FROM
    "commissions"."sfdc-w003v2-t002-teamnames" as s;
    
    
