---
view: '"commissions"."invoice-w002-t003-splits-order-number-prep"'
PK: identifier
Join on: idjoin (will create known duplicate rows)
---

```sql
WITH dealdata AS (
      SELECT
      identifier,
      ref_sales_order_number::character varying(200) AS ref_sales_order_number
      ,invoice_amount_local_currency
      FROM
      "public"."adwc_xxisc_invoice_order_details"
  ),
  splitdata AS (
      SELECT DISTINCT
      order_number::character varying(200) as order_number,
      sub_territory_id_dm,
      allocation

      FROM "override"."sheets_join invoice overrides_join_inv split order number"
      WHERE
      "QC Check unique PK" is true
      and sub_territory_id_dm is not null
  )
select
a.identifier::character varying (200) || ' - ' || b.sub_territory_id_dm::character varying (200) AS identifier,
a.identifier as idjoin,
a.ref_sales_order_number,
b.sub_territory_id_dm
,a.invoice_amount_local_currency
,b.allocation
,a.invoice_amount_local_currency * b.allocation AS invoice_amount_local_currency_ov
from
dealdata as a
inner join splitdata as b ON a.ref_sales_order_number = b.order_number
```
