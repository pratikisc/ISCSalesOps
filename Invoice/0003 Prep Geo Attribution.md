---
View: '"commissions"."invoice-w001-t003-geo-attribution"'
---

```sql

WITH geo_data AS (
    SELECT
        identifier,
        ship_to_country,
        ship_to_state,
        CASE
            WHEN ship_to_country IN ('CA', 'US') THEN ship_to_country || ' ' || left(ship_to_postal_code, 3)
            END AS ship_to_USCA,
        CASE
            WHEN ship_to_country IN ('CA', 'US') THEN ship_to_country || ' ' || upper(left(ship_to_state, 2))
            END AS ship_to_USCA_backup,
            
        CASE
            WHEN ship_to_country = 'AU' THEN ship_to_country || ' ' || REGEXP_SUBSTR(ship_to_postal_code,'([0-9]{1,4})')
            END AS ship_to_AU,
        CASE
            WHEN ship_to_country = 'AU' THEN salesrep_name
            END AS ship_to_AU_backup_salesrep_anz,
            
        CASE
            WHEN ship_to_country = 'CH' THEN ship_to_country || REGEXP_SUBSTR(ship_to_postal_code,'([0-9]{1,1})')
            END AS ship_to_CH,
        CASE
            WHEN operating_unit IN ('OU_APAC_CHINA', 'OU_APAC_SINGAPORE', 'OU_APAC_THAILAND') THEN salesrep_name
            END AS salesrep_name_asia_china
            
        
        FROM "commissions"."invoice-w001-t002-base-invoices" as a
    )

select
a.identifier,
coalesce (b.__sub_territory_id, f.__sub_territory_id, c.__sub_territory_id, d.__sub_territory_id, e.__sub_territory_id, g.__sub_territory_id, h.__sub_territory_id) as geo_sub_territory_id,
b.__sub_territory_id as id_usca,
c.__sub_territory_id as id_country,
d.__sub_territory_id as id_au,
e.__sub_territory_id as id_ch,
f.__sub_territory_id as id_salesrep_asia_china,
g.__sub_territory_id as id_usca_backup,
h.__sub_territory_id as id_salesrep_anz_backup,

a.ship_to_country,
a.ship_to_USCA,
a.ship_to_AU,
a.ship_to_CH,
a.salesrep_name_asia_china,
a.ship_to_AU_backup_salesrep_anz

from
geo_data AS a
LEFT JOIN "territory"."sheets_join_territory_join_geo_us  ca postal" AS b ON a.ship_to_USCA = b.__geo_category
LEFT JOIN "territory"."sheets_join_territory_join_geo_country" AS c ON a.ship_to_country = c.__country_code
LEFT JOIN "territory"."sheets_join_territory_join_geo_anz postal" AS d ON a.ship_to_AU = d.__country_postal4
LEFT JOIN "territory"."sheets_join_territory_join_geo_switzerland postal" AS e ON a.ship_to_CH = e.__geo_category
LEFT JOIN "territory"."sheets_join_territory_join oracle sales rep name" as f ON a.salesrep_name_asia_china = f.__sales_rep_name
LEFT JOIN "territory"."sheets_join_territory_join_geo_us  ca full states" as g ON a.ship_to_USCA_backup = g.__geo_category
LEFT JOIN "territory"."sheets_join_territory_join oracle sales rep name" as h ON a.ship_to_AU_backup_salesrep_anz = h.__sales_rep_name
```
