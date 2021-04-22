---
view: '"commissions"."reference-v2-all-sfdc-accounts-geo"'
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
a.geocategory,
b.__sub_territory_id as st_id_amer
from
dim_account a
inner join "territory"."sheets_join_territory_join_geo_us  ca postal" b
on a.geocategory = b.__geo_category

```
