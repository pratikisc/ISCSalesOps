---
view: '"commissions"."reference-sfdc-case-details-override"'
---

```sql

select
casenumber,
"unique pk check",
override_bookeddate
from
"override"."sheets_join sfdc case overrides_join_sfdc case details override"
where
"unique pk check" = true

```
