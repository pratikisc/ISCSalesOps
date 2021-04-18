---
View: '"commissions"."pos-w001-t002-geo-attr-v2"'
---

```sql

WITH data AS (
      select
      a.identifier,
      a.source_of_data,
      a.customer_name,
      a.ship_to_USCA,
      a.ship_to_country || ' ' || a.ship_to_state AS ship_to_USCA_fullstate,
      a.ship_to_customer,
      a.ship_to_postal_code,
      a.ship_to_state,
      a.ship_to_country,
      a.total_units_sold,
      a.invoice_amount_local_currency,
      a.invoice_currency_code,
      a.transaction_date_date,
      a.item_number,
      a.item_description
      from "commissions"."pos-w001-t001-base-import-v2" as a
)

select
      a.identifier,
      a.source_of_data,
      coalesce( b.__sub_territory_id, c.__sub_territory_id) AS sub_territory_dm,
      a.customer_name,
      a.ship_to_USCA,
      a.ship_to_customer,
      a.ship_to_postal_code,
      a.ship_to_state,
      a.ship_to_country,
      a.total_units_sold,
      a.invoice_amount_local_currency,
      a.invoice_currency_code,
      a.transaction_date_date,
      a.item_number,
      a.item_description
from data as a
LEFT JOIN "territory"."sheets_join_territory_join_geo_us  ca postal" AS b ON a.ship_to_USCA = b.__geo_category
LEFT JOIN "territory"."sheets_join_territory_join_geo_us  ca full states" as c ON a.ship_to_USCA_fullstate = c.__geo_category




```
