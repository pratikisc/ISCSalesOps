---
View: '"commissions"."invoice-w001-t002-base-invoices"'
---

```sql

SELECT
a.identifier,
a.source_of_data,
a.operating_unit,
coalesce(d.lob_allocation_override, a.lob_allocation) as lob_allocation,
a.high_level_lob_allocation,
a.low_level_lob_allocation,
a.line_of_business,
a.family,
coalesce ( b.__product_category,a.product_group ) as product_group,
a.product_type_group,
a.report_type_secondary,
a.contract_group,
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
a.ship_to_province,
a.ship_to_postal_code,
a.transaction_date_date,
a.invoice_amount_local_currency,
a.invoice_amount_usd,
a.invoice_currency_code,
a.total_units_sold,
a.key_account,
a.salesrep_name


FROM "public"."sheets_invoice details_jan" as a
LEFT JOIN "territory"."sheets_join_territory_join safer hw part numbers" as b on a.item_number = b.__item_number
LEFT JOIN "commissions"."invoice-w001-t001-exclusions" as c on a.identifier = c.identifier
LEFT JOIN "commissions"."invoice-w001-t0000-lob-cleanup" AS d on a.identifier = d.identifier

WHERE c.identifier is null

```
