---
view: '"commissions"."invoice-w001-t0000-a-union-invoices"'
---

```sql

SELECT
identifier,
source_of_data,
operating_unit,
lob_allocation,
high_level_lob_allocation,
low_level_lob_allocation,
line_of_business,
family,
product_group,
product_type_group,
report_type_secondary,
contract_group,
item_description,
invoice_number,
ref_sales_order_number,
customer_po_number,
item_number,
invoice_description,
customer_number,
customer_name,
ship_to_customer,
ship_to_customer_number,
ship_to_country,
ship_to_state,
ship_to_province,
ship_to_postal_code,
transaction_date_date,
invoice_amount_local_currency,
invoice_amount_usd,
invoice_currency_code,
total_units_sold,
key_account,
salesrep_name

FROM "public"."sheets_invoice details_jan"

UNION

SELECT
identifier,
source_of_data,
operating_unit,
lob_allocation,
high_level_lob_allocation,
low_level_lob_allocation,
line_of_business,
family,
product_group,
product_type_group,
report_type_secondary,
contract_group,
item_description,
invoice_number,
ref_sales_order_number,
customer_po_number,
item_number,
invoice_description,
customer_number,
customer_name,
ship_to_customer,
ship_to_customer_number,
ship_to_country,
ship_to_state,
ship_to_province,
ship_to_postal_code,
transaction_date_date,
invoice_amount_local_currency,
invoice_amount_usd,
invoice_currency_code,
total_units_sold,
null::character varying AS key_account,
salesrep_name

FROM "public"."sheets_historical invoice 2020 americas_sheet1"

```
