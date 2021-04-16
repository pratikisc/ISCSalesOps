---
view: '"commissions"."invoice-w001-t006-dm-union"'
note: Invoice, POS, Pronto; POS is excluded

---

```sql

SELECT
identifier::character varying (200),
source_of_data::character varying (200),
operating_unit::character varying (200),
lob_allocation::character varying (200),
high_level_lob_allocation::character varying (200),
low_level_lob_allocation::character varying (200),
line_of_business::character varying (200),
family::character varying (200),
product_group::character varying (200),
product_type_group::character varying (200),
report_type_secondary::character varying (200),
contract_group::character varying (200),
item_description::character varying (300),
invoice_number::character varying (200),
ref_sales_order_number::character varying (200),
customer_po_number::character varying (200),
item_number::character varying (200),
invoice_description::character varying (300),
customer_number::bigint,
customer_name::character varying (200),
ship_to_customer::character varying (200),
ship_to_customer_number::bigint,
ship_to_country::character varying (200),
ship_to_state::character varying (200),
ship_to_province::character varying (200),
ship_to_postal_code::character varying (200),
transaction_date_date::character varying (200),
invoice_amount_local_currency::double precision,
invoice_amount_usd::double precision,
invoice_currency_code::character varying (200),
total_units_sold::double precision,
key_account::character varying (200),
salesrep_name::character varying (200),
sub_territory_dm::bigint

from
"commissions"."invoice-w001-t003-joins"  -- Invoice 
where flag_pos_exclusion is null   -- POS Excluded


UNION

select  
      
identifier::character varying (200),
source_of_data::character varying (200),
null::character varying (200) as operating_unit,
'POS | Various'::character varying (200) as lob_allocation,
null::character varying (200) as high_level_lob_allocation,
null::character varying (200) as low_level_lob_allocation,
null::character varying (200) as line_of_business,
null::character varying (200) as family,
null::character varying (200) as product_group,
null::character varying (200) as product_type_group,
null::character varying (200) as report_type_secondary,
null::character varying (200) as contract_group,
item_description::character varying (300) as item_description,
null::character varying (200) as invoice_number,
null::character varying (200) as ref_sales_order_number,
null::character varying (200) as customer_po_number,
item_number::character varying (200) as item_number,
null::character varying (200) as invoice_description,
null::bigint as customer_number,
customer_name::character varying (200) as customer_name,
ship_to_customer::character varying (200) as ship_to_customer,
null::bigint as ship_to_customer_number,
ship_to_country::character varying (200) as ship_to_country,
ship_to_state::character varying (200) as ship_to_state,
null::character varying (200) as ship_to_province,
ship_to_postal_code::character varying (200) as ship_to_postal_code,
transaction_date_date::character varying (200) as transaction_date_date,
invoice_amount_local_currency::double precision as invoice_amount_local_currency,
null::double precision as invoice_amount_usd,
invoice_currency_code::character varying (200) as invoice_currency_code,
total_units_sold::double precision as total_units_sold,
null::character varying (200) as key_account,
null::character varying (200) as salesrep_name,
sub_territory_dm::bigint as sub_territory_dm     

from "commissions"."pos-w001-t002-geo-attr"  -- POS


UNION

select  
      
identifier::character varying (200),
source_of_data::character varying (200),
null::character varying (200) as operating_unit,
lob_allocation::character varying (200) as lob_allocation,
null::character varying (200) as high_level_lob_allocation,
null::character varying (200) as low_level_lob_allocation,
null::character varying (200) as line_of_business,
null::character varying (200) as family,
null::character varying (200) as product_group,
null::character varying (200) as product_type_group,
null::character varying (200) as report_type_secondary,
null::character varying (200) as contract_group,
null::character varying (300) as item_description,
null::character varying (200) as invoice_number,
null::character varying (200) as ref_sales_order_number,
null::character varying (200) as customer_po_number,
null::character varying (200) as item_number,
null::character varying (200) as invoice_description,
null::bigint as customer_number,
customer_name::character varying (200) as customer_name,
ship_to_customer::character varying (200) as ship_to_customer,
null::bigint as ship_to_customer_number,
null::character varying (200) as ship_to_country,
null::character varying (200) as ship_to_state,
null::character varying (200) as ship_to_province,
null::character varying (200) as ship_to_postal_code,
transaction_date_date::character varying (200) as transaction_date_date,
invoice_amount_local_currency::double precision as invoice_amount_local_currency,
null::double precision as invoice_amount_usd,
invoice_currency_code::character varying (200) as invoice_currency_code,
null::double precision as total_units_sold,
null::character varying (200) as key_account,
salesrep_name::character varying (200) as salesrep_name,
geo_sub_territory_id::bigint as sub_territory_dm     

from "commissions"."pronto-w001-t001-base"


```
