---
title: Transformation (Intermediate Steps)
description: 
Status: Interim View
---

```sql

WITH 
    cols as (
      select 'Team1' as c
      union all 
      select 'Team2' as c
      union all 
      select 'Team3' as c
      union all 
      select 'Team4' as c
      union all 
      select 'Team5' as c
      union all 
      select 'Team6' as c
      union all 
      select 'Team7' as c
      union all 
      select 'Team8' as c
      )
select 
    casenumber,
    c "Team",
    CASE c 
        WHEN 'Team1' THEN "Team1" 
        WHEN 'Team2' THEN "Team2"
        WHEN 'Team3' THEN "Team3"
        WHEN 'Team4' THEN "Team4"
        WHEN 'Team5' THEN "Team5"
        WHEN 'Team6' THEN "Team6"
        WHEN 'Team7' THEN "Team7"
        WHEN 'Team8' THEN "Team8"
        ELSE NULL
    END as "TeamValue"

from "sfdc-w003-t001-splittable" cross join cols
order by casenumber, team asc;


```

## View Name: `"sfdc-w003-t001-splittable"`

| Column | Description |
| --- | --- |
|`All Column Descriptions`|   |
