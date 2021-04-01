---
View: '"commissions"."invoice-w001-t0000-lob-cleanup"'
---
```sql

-- Clean up Unknown LOBs
SELECT
identifier,
CASE
    WHEN lower(invoice_description) like '%rent%' then 'Rent'
    WHEN lower(invoice_description) like '%distributor%' then 'Distributor Commissions'
    WHEN lower(invoice_description) like '%rebate%' then 'Distributor Commissions'
    WHEN lower(invoice_description) like '%repair%' then 'Service/Parts/Accessories'
    WHEN lower(invoice_description) like '%support%' then 'Service/Parts/Accessories'
    WHEN lower(invoice_description) like '%sensors%' then 'Service/Parts/Accessories'
    WHEN lower(invoice_description) like '%period%' then 'iNet'
    WHEN product_group = 'SAFER' then 'Hardware'
    WHEN b.__product_category = 'SAFER' then 'Hardware'
    WHEN report_type_secondary = 'NON-INET' then 'Hardware'
    WHEN report_type_secondary = 'SERVICE' then 'Service/Parts/Accessories'
    END as lob_allocation_override
    

FROM "public"."sheets_invoice details_jan" as a
LEFT JOIN "territory"."sheets_join_territory_join safer hw part numbers" as b on a.item_number = b.__item_number

WHERE
lob_allocation = 'Unknown'

```
