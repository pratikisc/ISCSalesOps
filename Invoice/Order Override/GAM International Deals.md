---
view: '"commissions"."invoice-w002-t002-gam-international-deals-order-number-override"'
PK: identifier
Join on: idjoin (will create known duplicate rows)
---

### Only for 100% overrides, not for splits

```sql

WITH dealdata AS (
      SELECT
      identifier,
      ref_sales_order_number::character varying(200) AS ref_sales_order_number
      ,invoice_amount_local_currency
      FROM
      "public"."sheets_invoice details_jan"
  ),
  
  ovdata AS (
      SELECT
      order_number::character varying(200) as order_number,
      sub_territory_id_gam

      FROM "override"."sheets_join invoice overrides_join_inv order ov gam int'l deals"
      WHERE
      "QC Check unique PK" is true
      and sub_territory_id_gam is not null
  )
  
select
a.identifier,
a.ref_sales_order_number,
b.sub_territory_id_gam
,a.invoice_amount_local_currency
from
dealdata as a
inner join ovdata as b ON a.ref_sales_order_number = b.order_number

```
