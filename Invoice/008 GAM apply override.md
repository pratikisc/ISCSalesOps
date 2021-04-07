---
view: '"commissions"."invoice-w001-t008-gam-apply-override"'
---


```sql

SELECT
a.identifier,
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
a.invoice_amount_local_currency,
a.invoice_amount_usd,
a.invoice_currency_code,
a.total_units_sold,
a.key_account,
a.salesrep_name,

a.sub_territory_dm,
a.sub_territory_kam,
a.sub_territory_safer,
a.sub_territory_fixed,
a.sub_territory_amer_rent,
coalesce( b.sub_territory_id_gam, a.sub_territory_gam, a.sub_territory_safer) as sub_territory_gam,
a.sub_territory_psm_named,
a.sub_territory_psm_geo,
a.flag_pos_exclusion


FROM "commissions"."invoice-w001-t003-joins" a
LEFT JOIN "commissions"."invoice-w002-t002-gam-order-number-override-prep" b ON a.identifier = b.identifier


```
