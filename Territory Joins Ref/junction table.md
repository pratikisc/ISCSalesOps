```
view:'"territory"."junction-emp-terr-v2"'
```


```sql
SELECT DISTINCT
    
__sub_territory_id || ' - ' || __ciq__id AS id,
__sub_territory_id,
__ciq__id,
__ftv__id,
"employee name",
"sub territory",
"plan category",
coverage,
cast("effective end date" as date) as effective_end_date,
cast("effective start date" as date) as effective_start_date
FROM
territory."sheets_join_territory_employees to territories"
ORDER BY
__ciq__id,
"sub territory"

```
