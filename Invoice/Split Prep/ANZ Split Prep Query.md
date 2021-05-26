---
view: '"commissions"."invoice-w002-t001-anz-split-prep"'
PK: identifier
Join on: idjoin (will create known duplicate rows)
---


Included:
- AGI
- Airmet
- Gastech

```sql

SELECT 
a.identifier as idjoin,
a.identifier::character varying (200) || ' - ' || c.__sub_territory_id::character varying (200) AS identifier,
b.__distributor_roll_up_id,
a.customer_name,
a.ref_sales_order_number,
c.__sub_territory_id,
c.__allocation,
a.invoice_amount_local_currency,
c.__allocation * a.invoice_amount_local_currency AS invoice_amount_local_currency_ovr
 
FROM "public"."adwc_xxisc_invoice_order_details" a
INNER JOIN "territory"."sheets_join_territory_join_inv named distributor accounts" b ON a.customer_number = b.__oracle_account_number
LEFT JOIN "override"."sheets_join invoice overrides_join anz split" c ON b.__distributor_roll_up_id = c.__dist_id
WHERE b.__distributor_roll_up_id IN ('D015-AIRMET','D016-GASTECH', 'D001-AGI')
ORDER BY 1


```
