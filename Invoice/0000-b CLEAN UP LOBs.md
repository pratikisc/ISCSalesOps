---
Old View: '"commissions"."invoice-w001-t0000-lob-cleanup"'
New View: '"commissions"."invoice-w001-t0000-lob-cleanup-v2"'
---
new
```sql


-- Clean up Unknown LOBs
SELECT
identifier,
CASE
    WHEN lower(invoice_description) like '%rent%' then 'Rent'::character varying(25)
    WHEN lower(invoice_description) like '%distributor%' then 'Distributor Commissions'::character varying(25)
    WHEN lower(invoice_description) like '%rebate%' then 'Distributor Commissions'::character varying(25)
    WHEN lower(invoice_description) like '%repair%' then 'Service/Parts/Accessories'::character varying(25)
    WHEN lower(invoice_description) like '%support%' then 'Service/Parts/Accessories'::character varying(25)
    WHEN lower(invoice_description) like '%sensors%' then 'Service/Parts/Accessories'::character varying(25)
    WHEN lower(invoice_description) like '%period%' then 'iNet'::character varying(25)
    WHEN product_group = 'SAFER' then 'Hardware'::character varying(25)
    WHEN b.__product_category = 'SAFER' then 'Hardware'::character varying(25)
    WHEN report_type_secondary = 'NON-INET' then 'Hardware'::character varying(25)
    WHEN report_type_secondary = 'SERVICE' then 'Service/Parts/Accessories'::character varying(25)
    WHEN report_type_secondary = 'ATO' then 'Hardware'::character varying(25)
    WHEN report_type_secondary = 'RENTAL' then 'Rent'::character varying(25)
    WHEN report_type_secondary = 'INET' then 'iNet'::character varying(25)
    END as lob_allocation_override
    

FROM "public"."adwc_xxisc_invoice_order_details" as a
LEFT JOIN "territory"."sheets_join_territory_join safer hw part numbers" as b on a.item_number = b.__item_number

WHERE
(
lob_allocation = 'Unknown' or
lob_allocation is null
)

and
(
source_of_data = 'Invoice' and
invoice_amount_usd <> 0
)


```

old
```sql

-- Clean up Unknown LOBs
SELECT
identifier,
CASE
    WHEN lower(invoice_description) like '%rent%' then 'Rent'::character varying(25)
    WHEN lower(invoice_description) like '%distributor%' then 'Distributor Commissions'::character varying(25)
    WHEN lower(invoice_description) like '%rebate%' then 'Distributor Commissions'::character varying(25)
    WHEN lower(invoice_description) like '%repair%' then 'Service/Parts/Accessories'::character varying(25)
    WHEN lower(invoice_description) like '%support%' then 'Service/Parts/Accessories'::character varying(25)
    WHEN lower(invoice_description) like '%sensors%' then 'Service/Parts/Accessories'::character varying(25)
    WHEN lower(invoice_description) like '%period%' then 'iNet'::character varying(25)
    WHEN product_group = 'SAFER' then 'Hardware'::character varying(25)
    WHEN b.__product_category = 'SAFER' then 'Hardware'::character varying(25)
    WHEN report_type_secondary = 'NON-INET' then 'Hardware'::character varying(25)
    WHEN report_type_secondary = 'SERVICE' then 'Service/Parts/Accessories'::character varying(25)
    WHEN report_type_secondary = 'ATO' then 'Hardware'::character varying(25)
    WHEN report_type_secondary = 'RENTAL' then 'Rent'::character varying(25)
    WHEN report_type_secondary = 'INET' then 'iNet'::character varying(25)
    END as lob_allocation_override
    

FROM "commissions"."invoice-w001-t0000-a-union-invoices" as a
LEFT JOIN "territory"."sheets_join_territory_join safer hw part numbers" as b on a.item_number = b.__item_number

WHERE
lob_allocation = 'Unknown' or
lob_allocation is null

```
