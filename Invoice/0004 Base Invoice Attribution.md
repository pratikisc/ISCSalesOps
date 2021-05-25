---
View: '"commissions"."invoice-w001-t003-attribution"'
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
    WHEN
        (
            line_of_business = 'GASTRON' or
            lower(customer_po_number) like '%-fixed-%'
        )
        and
        (
        operating_unit = 'OU_APAC_CHINA'
        )
    THEN 7015::bigint
    END AS sub_territory_fixed,


-- Americas Rental Sales as 7036
CASE
    WHEN
    a.lob_allocation = 'Rent' and
    b.geo_sub_territory_id IN (
        select id from "territory"."sheets_join_territory_territories" where region = 'AMER'
        )
    THEN 7036::bigint -- Americas Rental to Jason Wright Sub Territory Number
    END AS sub_territory_amer_rent,
    
-- DM Sub Territory join, includes Territory Overrides for Known Named Accounts (ex: Neumann Meridian Trading)
coalesce(i.sub_territory_id, b.geo_sub_territory_id) AS sub_territory_dm,


-- KAM Join
CASE WHEN 
        b.geo_sub_territory_id IN (
            select id from "territory"."sheets_join_territory_territories" where region = 'EMEA'
        )
     THEN
     coalesce( c.__sub_territory_id, f.__kam_sub_territory_id, g.__kam_sub_territory_id)
     
     WHEN
        a.operating_unit like '%APAC%'
     THEN
     c.__sub_territory_id
     
     END AS sub_territory_kam,  -- KAM logic


-- GAM Join
    CASE WHEN
    b.geo_sub_territory_id IN (
        select id from "territory"."sheets_join_territory_territories" where region = 'AMER'
        )
    THEN
    coalesce( d.__gam_sub_territory_id, e.__gam_sub_territory_id)
    END AS sub_territory_gam, -- GAM Account joins


-- PSM Americas Named Accounts
CASE WHEN
    b.geo_sub_territory_id IN (
        select id from "territory"."sheets_join_territory_territories" where region = 'AMER' and "sub region" <> 'Latin America'
        )
     THEN
    h.__sub_territory_id
    END AS sub_territory_psm_named,  -- PSM Named Accounts in Americas
-- PSM Americas Geo
CASE WHEN
    (
        b.geo_sub_territory_id IN (
        select id from "territory"."sheets_join_territory_territories" where region = 'AMER' and "sub region" <> 'Latin America'
        )
    )
    and
    (
        h.__oracle_account_number is not null    -- Only Distributor Accounts   
    )
    THEN
    b.geo_sub_territory_id
    END AS sub_territory_psm_geo,

-- Americas POS Re-Distribution

CASE WHEN
    (
        b.geo_sub_territory_id IN (
        select id from "territory"."sheets_join_territory_territories" where region = 'AMER' and "sub region" <> 'Latin America'
        )
    )
    and h.__distributor_roll_up_id IN ('D012-CONN',
                                       'D011-DNOW',
                                       'D002-DXP',
                                       'D004-HAZ',
                                       'D005-LEV',
                                       'D003-MES',
                                       'D006-NSI',
                                       'D007-ORR',
                                       'D008-UCIS',
                                       'D010-VACA',
                                       'D009-VAUS'
                                       )
    and
    (
        a.lob_allocation IN ('Hardware', 'Rent')
        or
        (
            a.lob_allocation = 'Service/Parts/Accessories'
            and
            a.line_of_business <> 'SERVICE'
        )
    )
    THEN 'POS-Exclude'::character varying (11)
    END AS flag_pos_exclusion
    
    
    

FROM "commissions"."invoice-w001-t002-base-invoices-v2" as a
LEFT JOIN "commissions"."invoice-w001-t003-geo-attribution" AS b ON a.identifier = b.identifier
LEFT JOIN "territory"."sheets_join_territory_join oracle key_account" as c ON a.key_account = c.__key_account
LEFT JOIN "territory"."sheets_join_territory_join_inv gam accounts" as d ON a.customer_number = d.__account_number
LEFT JOIN "territory"."sheets_join_territory_join_inv gam accounts" as e ON a.ship_to_customer_number = e.__account_number
LEFT JOIN "territory"."sheets_join_territory_join kam accounts" as f ON a.customer_number = f.__account_number
LEFT JOIN "territory"."sheets_join_territory_join kam accounts" as g ON a.ship_to_customer_number = g.__account_number
LEFT JOIN "territory"."sheets_join_territory_join_inv named distributor accounts" as h ON a.customer_number = h.__oracle_account_number
LEFT JOIN "territory"."sheets_join_territory_ship to territory override named account" as i ON a.customer_number = i.account_number

```
