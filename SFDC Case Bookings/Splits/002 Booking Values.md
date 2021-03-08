
```sql
WITH 
    colv as (
      select 'Value1' as cv
      union all 
      select 'Value2' as cv
      union all 
      select 'Value3' as cv
      union all 
      select 'Value4' as cv
      union all 
      select 'Value5' as cv
      union all 
      select 'Value6' as cv
      union all 
      select 'Value7' as cv
      union all 
      select 'Value8' as cv
      )
select 
    casenumber,
    cv "Value",
    CASE cv 
        WHEN 'Value1' THEN "Value1" 
        WHEN 'Value2' THEN "Value2"
        WHEN 'Value3' THEN "Value3"
        WHEN 'Value4' THEN "Value4"
        WHEN 'Value5' THEN "Value5"
        WHEN 'Value6' THEN "Value6"
        WHEN 'Value7' THEN "Value7"
        WHEN 'Value8' THEN "Value8"
        ELSE NULL
    END as "ValueValue"

from "sfdc-w003-t001-splittable" cross join colv
order by casenumber, Value asc;

```

## View Name: `"sfdc-w003-t002-bookingvalues"`

| Column | Description |
| --- | --- |
|`All Column Descriptions`| All column descriptions are in the final view  |


