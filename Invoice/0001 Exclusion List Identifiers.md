---
view: '"commissions"."invoice-w001-t001-exclusions"'
---

```sql

-- Exclusion list with Identifiers

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
ship_to_postal_code,
transaction_date_date,
invoice_amount_local_currency,
invoice_amount_usd,
invoice_currency_code,
key_account,
salesrep_name


FROM "public"."sheets_invoice details_jan" as a
LEFT JOIN "territory"."sheets_join_territory_exclude_inv account exclusion list" AS b ON a.customer_number = b.__account_number
WHERE
lower(customer_name) like '%industrial scientific%'
or product_type_group = 'PSC'
or b.id is not null
or lob_allocation in ('PSC', 'Intercompany', 'Distributor Commissions')
or (
    lob_allocation = 'iNet' and
    line_of_business IN ('SERVICE', 'NO_LOB')
    )

```
