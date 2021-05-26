---
view: '"commissions"."invoice-w002-t002-gam-order-number-override-prep-v2"'
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
      "public"."adwc_xxisc_invoice_order_details"
  ),
  
  ovdata AS (
      SELECT
      order_number::character varying(200) as order_number,
      sub_territory_id_gam

      FROM "override"."sheets_join invoice overrides_join_inv order number overrides"
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
