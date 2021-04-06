---
View:
---

```sql

SELECT
a.identifier,
a.ship_to_postal_code,
a.ship_to_country,
left(a.ship_to_postal_code, 3) as ship_to_postal_USCA,
REGEXP_SUBSTR(a.ship_to_postal_code,'([0-9]{1,4})') as postal4,
REGEXP_SUBSTR(a.ship_to_postal_code,'([0-9]{1,1})') as postal1,
REGEXP_SUBSTR(a.ship_to_postal_code,'[0-9]+') as postalnum



FROM "commissions"."invoice-w001-t002-base-invoices" as a
```
