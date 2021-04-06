---
View: '"commissions"."invoice-w001-t003-asia-china-attribution"'
---
```sql

SELECT
a.identifier,
a.operating_unit,
a.lob_allocation,
a.line_of_business,
a.family,
a.product_group,
a.customer_po_number,
a.item_number,
a.key_account,
a.salesrep_name,
a.ship_to_postal_code,
a.ship_to_country,
a.customer_number,
a.ship_to_customer_number,
a.ref_sales_order_number,

-- SAFER Hardware Sales

CASE
    WHEN line_of_business = 'HARDWARE' and product_group = 'SAFER' and operating_unit like '%_APAC_%' THEN 7013::bigint -- Robin Kang SAFER
    WHEN line_of_business = 'HARDWARE' and product_group = 'SAFER' THEN 7070::bigint -- Catch all Fred Humbert
    END AS sub_territory_safer,


-- Fixed System China as 7015

CASE
    WHEN line_of_business = 'GASTRON' or lower(customer_po_number) like '%-fixed-%' THEN 7015::bigint
    END AS sub_territory_fixed,


-- Americas Rental Sales as 7036
CASE
    WHEN line_of_business = 'Rent' and operating_unit like '%_AMER_%' THEN 7036::bigint -- Americas Rental to Jason Wright
    END AS sub_territory_amer_rent,
    
    
b.__sub_territory_id AS sub_territory_dm,
c.__sub_territory_id AS sub_territory_kam -- KEY_ACCOUNT field in Oracle Invoice

FROM "commissions"."invoice-w001-t002-base-invoices" as a
LEFT JOIN "commissions"."invoice-w001-t003-geo-attribution" AS b ON a.identifier = b.identifier
LEFT JOIN "territory"."sheets_join_territory_join oracle key_account" as c ON a.key_account = c.__key_account

```
