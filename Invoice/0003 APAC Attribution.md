
```sql

-- APAC SAFER and China Fixed Systems, Sales Rep Name Sub territory
SELECT
a.identifier,
a.operating_unit,
a.line_of_business,
a.family,
a.product_group,
a.customer_po_number,
a.item_number,
a.key_account,
a.salesrep_name,

-- Hard coded to '7013' from territory file as SAFER Sales

CASE
    WHEN line_of_business = 'HARDWARE' and product_group = 'SAFER' THEN 7013::bigint
    END AS sub_territory_safer,


-- Fixed System as '7015'

CASE
    WHEN line_of_business = 'GASTRON' or lower(customer_po_number) like '%-fixed-%' THEN 7015::bigint
    END AS sub_territory_fixed

FROM "commissions"."invoice-w001-t002-base-invoices" as a
LEFT JOIN "territory"."sheets_join_territory_join oracle sales rep name" as b ON a.salesrep_name = b.__sales_rep_name
LEFT JOIN "territory"."sheets_join_territory_join oracle key_account" as c ON a.key_account = c.__key_account
WHERE operating_unit like '%_APAC_%'
order by sub_territory_fixed

```
