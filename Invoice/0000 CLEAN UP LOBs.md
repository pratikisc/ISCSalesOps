---
View: '"commissions"."invoice-w001-t000-lob-cleanup"'
---
```sql

-- Clean up Unknown LOBs
SELECT
invoice_description,
CASE
    WHEN lower(invoice_description) like '%rent%' then 'Rent'
    WHEN lower(invoice_description) like '%distributor%' then 'Distributor Commissions'
    WHEN product_group = 'SAFER' then 'Hardware'
    WHEN report_type_secondary = 'NON-INET' then 'Hardware'
    WHEN report_type_secondary = 'SERVICE' then 'Service/Parts/Accessories'
    END as lob_allocation_override
    

FROM "public"."sheets_invoice details_jan" as a
WHERE
lob_allocation = 'Unknown'
and 
    (
        lower(invoice_description) like '%rent%'
        or lower(invoice_description) like '%distributor%'
    )
```
