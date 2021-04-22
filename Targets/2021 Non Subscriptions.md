---
view: "targets"."2021-non-subscriptions-monthly"
---

```sql
select
a.year,
a."sub territory id(s)",
a.target_type,
b.type as seasonality_type,
a.annual_value * b.weight as monthly_value
from
"territory"."sheets_join_territory_targets" a
left join "territory"."sheets_join_territory_seasonality" b ON a.year = b.year
where
target_type = 'Non Subscription'
and b.type = 'Non Subscription'


```sql
