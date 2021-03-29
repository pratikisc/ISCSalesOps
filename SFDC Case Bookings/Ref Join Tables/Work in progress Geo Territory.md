```
select
id,
shippingcountry,
left(shippingpostalcode,3) postal3,
REGEXP_SUBSTR(shippingpostalcode,'([0-9]{1,4})') as postal4
from
salesforce_account
where shippingcountry in ('CH','ch')
