---
view: '"commissions"."invoice-w002-t004-invoice-number-override-prep"'
PK: identifier
Join on: idjoin (will create known duplicate rows)
---

### Only for 100% overrides, not for splits

```sql

WITH dealdata AS (
      SELECT
      identifier,
      invoice_number::character varying(200) AS invoice_number
      ,invoice_amount_local_currency
      FROM
      "public"."adwc_xxisc_invoice_order_details"
      where
      invoice_amount_usd <> 0
  ),
  
  ovdata AS (
      SELECT
      invoice_number::character varying(200) as invoice_number,
      sub_territory_id_dm

      FROM "override"."sheets_join invoice overrides_join_inv invoice number overrides"
      WHERE
      "QC Check unique PK" is true
      and sub_territory_id_dm is not null
  )
  
select
a.identifier,
a.invoice_number,
b.sub_territory_id_dm
,a.invoice_amount_local_currency
from
dealdata as a
inner join ovdata as b ON a.invoice_number = b.invoice_number

```
