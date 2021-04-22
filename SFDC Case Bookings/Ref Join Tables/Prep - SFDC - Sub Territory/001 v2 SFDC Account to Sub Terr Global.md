---
view: '"territory"."reference-v2-all-sfdc-accounts-geo"'
---

```sql
with dim_account as (
    select
    a.id,
    coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) as country,
    coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) || ' ' || coalesce( left(a.shippingpostalcode,3), left(a.billingpostalcode,3) ) uscageocategory,
    
    CASE
            WHEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) IN ('CA', 'US') THEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) || ' ' || coalesce( left(a.shippingpostalcode,3), left(a.billingpostalcode,3) )
            END AS ship_to_USCA,
        CASE
            WHEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) IN ('CA', 'US') THEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) || ' ' || upper(left(shippingstate, 2))
            END AS ship_to_USCA_backup,
            
        CASE
            WHEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) = 'AU' THEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) || ' ' || REGEXP_SUBSTR(shippingpostalcode,'([0-9]{1,4})')
            END AS ship_to_AU,
                   
        CASE
            WHEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) = 'CH' THEN coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) || REGEXP_SUBSTR( coalesce( left(a.shippingpostalcode,3), left(a.billingpostalcode,3) ) ,'([0-9]{1,1})')
            END AS ship_to_CH
    
    from
    salesforce_account a
    
    )

select
a.id,
coalesce (b.__sub_territory_id, c.__sub_territory_id, d.__sub_territory_id, e.__sub_territory_id, g.__sub_territory_id) as geo_sub_territory_id,
a.country,
a.ship_to_USCA,
a.ship_to_USCA_backup,
a.ship_to_AU,
a.ship_to_CH,
b.__sub_territory_id as st_id_amer
from
dim_account a
LEFT JOIN "territory"."sheets_join_territory_join_geo_us  ca postal" AS b ON a.ship_to_USCA = b.__geo_category
LEFT JOIN "territory"."sheets_join_territory_join_geo_country" AS c ON a.country = c.__country_code
LEFT JOIN "territory"."sheets_join_territory_join_geo_anz postal" AS d ON a.ship_to_AU = d.__country_postal4
LEFT JOIN "territory"."sheets_join_territory_join_geo_switzerland postal" AS e ON a.ship_to_CH = e.__geo_category
LEFT JOIN "territory"."sheets_join_territory_join_geo_us  ca full states" as g ON a.ship_to_USCA_backup = g.__geo_category

```
