---
view: '"commissions"."reference-sfdc-case-details-override-v2"'
---

```sql

select
casenumber,
"unique pk check",
override_bookeddate,
override_mrrchangelocal,
override_contractlength
from
"override"."sheets_join sfdc case overrides_join_sfdc case details override"
where
"unique pk check" = true

```
