---
old view: '"commissions"."invoice-w001-t001-exclusions-v2"'
new view: '"commissions"."invoice-w001-t001-exclusions-v3"'
---

### All Exclusion Criteria


new
```sql
SELECT
a.identifier,
a.source_of_data,
a.operating_unit,
coalesce(c.lob_allocation_override::text, a.lob_allocation::text) as lob_allocation,
a.high_level_lob_allocation,
a.low_level_lob_allocation,
a.line_of_business,
a.family,
a.product_group,
a.product_type_group,
a.report_type_secondary,
a.item_description,
a.invoice_number,
a.ref_sales_order_number,
a.customer_po_number,
a.item_number,
a.invoice_description,
a.customer_number,
a.customer_name,
a.ship_to_customer,
a.ship_to_customer_number,
a.ship_to_country,
a.ship_to_state,
a.ship_to_postal_code,
a.transaction_date_date,
a.invoice_amount_local_currency,
a.invoice_amount_usd,
a.invoice_currency_code,
a.key_account,
a.salesrep_name


FROM "commissions"."invoice-w001-t0000-raw-invoice-adwc" as a
LEFT JOIN "territory"."sheets_join_territory_exclude_inv account exclusion list" AS b ON a.customer_number = b.__account_number
LEFT JOIN "commissions"."invoice-w001-t0000-lob-cleanup-v2" AS c ON a.identifier = c.identifier
WHERE
lower(a.customer_name) like '%industrial scientific%'
or lower(a.customer_name) like '%intelex%'
or a.product_type_group = 'PSC'
or b.id is not null
or a.lob_allocation in ('PSC', 'Intercompany', 'Distributor Commissions', 'Not Margin Account', 'iNet')
or c.lob_allocation_override in ('Distributor Commissions', 'iNet')

```


old
```sql

-- Exclusion list with Identifiers

SELECT
a.identifier,
a.source_of_data,
a.operating_unit,
coalesce(c.lob_allocation_override::text, a.lob_allocation::text) as lob_allocation,
a.high_level_lob_allocation,
a.low_level_lob_allocation,
a.line_of_business,
a.family,
a.product_group,
a.product_type_group,
a.report_type_secondary,
a.item_description,
a.invoice_number,
a.ref_sales_order_number,
a.customer_po_number,
a.item_number,
a.invoice_description,
a.customer_number,
a.customer_name,
a.ship_to_customer,
a.ship_to_customer_number,
a.ship_to_country,
a.ship_to_state,
a.ship_to_postal_code,
a.transaction_date_date,
a.invoice_amount_local_currency,
a.invoice_amount_usd,
a.invoice_currency_code,
a.key_account,
a.salesrep_name


FROM "commissions"."invoice-w001-t0000-a-union-invoices" as a
LEFT JOIN "territory"."sheets_join_territory_exclude_inv account exclusion list" AS b ON a.customer_number = b.__account_number
LEFT JOIN "commissions"."invoice-w001-t0000-lob-cleanup" AS c ON a.identifier = c.identifier
WHERE
lower(a.customer_name) like '%industrial scientific%'
or a.product_type_group = 'PSC'
or b.id is not null
or a.lob_allocation in ('PSC', 'Intercompany', 'Distributor Commissions', 'Not Margin Account', 'iNet')
or c.lob_allocation_override in ('Distributor Commissions', 'iNet')




```
