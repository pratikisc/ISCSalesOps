---
view: '"commissions"."pos-w001-t001-base-import-v2"'
---

`current version`

```sql
select
identifier,
source_of_data,
customer_name,
ship_to_country || ' ' || left(ship_to_postal_code, 3) AS ship_to_USCA,
ship_to_customer,
ship_to_postal_code,
ship_to_state,
ship_to_country,
total_units_sold,
invoice_amount_local_currency,
invoice_currency_code,
transaction_date_date,
item_number,
item_description
from
"public"."sheets_pos master_pos master"
where invoice_amount_local_currency <> 0

UNION

select
identifier,
source_of_data,
customer_name,
ship_to_country || ' ' || left(ship_to_postal_code, 3) AS ship_to_USCA,
ship_to_customer,
ship_to_postal_code,
ship_to_state,
ship_to_country,
total_units_sold,
invoice_amount_local_currency,
invoice_currency_code,
transaction_date_date,
item_number,
null::character varying as item_description

FROM "public"."sheets_pos historical_pos hist"
where invoice_amount_local_currency <> 0
```




`version 1`

```sql

select
identifier,
source_of_data,
customer_name,
ship_to_country || ' ' || left(ship_to_postal_code, 3) AS ship_to_USCA,
ship_to_customer,
ship_to_postal_code,
ship_to_state,
ship_to_country,
total_units_sold,
invoice_amount_local_currency,
invoice_currency_code,
transaction_date_date,
    
item_number,
item_description
from
"public"."sheets_pos master_pos master"

```
