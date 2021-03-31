---
view: '"commissions"."reference-sfdc account and ship to amer sub terr"'
'!!!' This view allocates the sub territory id to the DM query
---

```sql

-- US ad CA reference table (used in partner bookings)

with dim_account as (
    select
    a.id,
    coalesce( upper(a.shippingcountry), upper(a.billingcountry) ) || ' ' || coalesce( left(a.shippingpostalcode,3), left(a.billingpostalcode,3) ) geocategory
    from
    salesforce_account a
    where a.shippingcountry in ('US','CA', 'us', 'ca', null)
    )
select
a.id,
a.geocategory,
b.__sub_territory_id as st_id_amer
from
dim_account a
inner join "territory"."sheets_join_territory_join_geo_us  ca postal" b
on a.geocategory = b.__geo_category
