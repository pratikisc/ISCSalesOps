---
View: sfdc-contract-case-w006-t001-special-terms
---

```

WITH splist as (
            select
              id,
              'Auto-Renewal' as special_terms_list__c,
              parent_contract__c
              from
              salesforce_contract
              where
              special_terms_list__c like '%Auto-Renewal%'
           )
           
select
a.casenumber,
a.contract__c,
b.special_terms_list__c,
b.parent_contract__c,
a.booked_date__c,
a.finance_sub_status__c
from
salesforce_case AS a
inner join splist AS b ON a.contract__c = b.id
where a.finance_sub_status__c = 'Booked'
