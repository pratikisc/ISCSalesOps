---
view: '"commissions"."pronto-w001-t001-base"'
---

```sql

select
identifier,
lob_allocation,
salesrep_name,
geo_sub_territory_id,
source_of_data,
customer_name,
ship_to_customer,
invoice_amount_local_currency,
invoice_currency_code,
transaction_date_date
    
from
"public"."sheets_pronto_pronto"

```
