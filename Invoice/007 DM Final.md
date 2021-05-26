---
view: '"commissions"."invoice-w001-t007-dm-final"'
---

### Notes:
- Do not use Invoice Amount USD (Splits not accounted for) 

```sql

SELECT

coalesce( d.identifier, b.identifier, a.identifier) AS identifier,
a.source_of_data,
a.operating_unit,
a.lob_allocation,
a.high_level_lob_allocation,
a.low_level_lob_allocation,
a.line_of_business,
a.family,
a.product_group,
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
coalesce ( d.invoice_amount_local_currency_ov,b.invoice_amount_local_currency_ovr, a.invoice_amount_local_currency) AS invoice_amount_local_currency,
a.invoice_amount_usd,
a.invoice_currency_code,
a.total_units_sold,
a.key_account,
a.salesrep_name,
coalesce( d.sub_territory_id_dm, c.sub_territory_id_dm, e.sub_territory_id_dm, b.__sub_territory_id, a.sub_territory_dm) as sub_territory_dm

from
"commissions"."invoice-w001-t006-dm-union" AS a
LEFT JOIN "commissions"."invoice-w002-t001-anz-split-prep-v2" AS b ON a.identifier = b.idjoin
LEFT JOIN "commissions"."invoice-w002-t002-order-number-override-prep-v2" AS c ON a.identifier = c.identifier
LEFT JOIN "commissions"."invoice-w002-t003-splits-order-number-prep-v2" AS d ON a.identifier = d.idjoin
LEFT JOIN "commissions"."invoice-w002-t004-invoice-number-override-prep" AS e ON a.identifier = e.identifier

```
