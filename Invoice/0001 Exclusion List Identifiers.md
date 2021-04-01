---
view: '"commissions"."invoice-w001-t001-exclusions"'
---

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


FROM "public"."sheets_invoice details_jan" as a
LEFT JOIN "territory"."sheets_join_territory_exclude_inv account exclusion list" AS b ON a.customer_number = b.__account_number
LEFT JOIN "commissions"."invoice-w001-t0000-lob-cleanup" AS c ON a.identifier = c.identifier
WHERE
lower(a.customer_name) like '%industrial scientific%'
or a.product_type_group = 'PSC'
or b.id is not null
or a.lob_allocation in ('PSC', 'Intercompany', 'Distributor Commissions', 'Not Margin Account')
or c.lob_allocation_override = 'Distributor Commissions'
or (
    a.lob_allocation = 'iNet' and
    a.line_of_business = 'SERVICE'
    )
or (
    c.lob_allocation_override = 'iNet' and
    a.line_of_business = 'SERVICE'
    )
or
    (
    a.lob_allocation = 'iNet' and
    a.line_of_business = 'NO_LOB' and
    lower(a.invoice_description) not like '%termination%'
    )
 or
    (
    c.lob_allocation_override = 'iNet' and
    a.line_of_business = 'NO_LOB' and
    lower(a.invoice_description) not like '%termination%'
    )
    
 




```
