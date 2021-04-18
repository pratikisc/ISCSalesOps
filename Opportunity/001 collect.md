---
view: '"commissions"."opp-w001-t001-mdr"'
---

```sql

select
a.id
, d.sub_territory_id
, a.mdr__c
, b.full_name__c as mdr
, a.name
, a.createddate
, a.closedate
, a.lastmodifieddate
, a.leadsource
, a.web_source_detail__c
, a.po_number__c
, a.oracle_order_number__c
, a.oracle_contract_number__c
, a.ownerid
, c.full_name__c as opp_owner
, a.opportunity_number__c
, a.type
, a.solution_type__c
, a.converted_amount__c as total_converted_amount_corporate
, a.term_length__c
, case when a.solution_type__c like '%iNet%' then 'iNet'
       else 'Non-iNet'
       END AS mdr_plan_category
, case when coalesce(a.Term_Length__c, 1) <= 12 then a.converted_amount__c
       else ( a.converted_amount__c / coalesce(a.Term_Length__c, 1) ) * 12
       END AS annual_value_usd_corporate
, a.Annual_Value2__c as annual_amount_local_currency
, a.CurrencyIsoCode
, stagename
from sfdc_opportunity as a
left join salesforce_user as b ON b.id = a.mdr__c
left join salesforce_user as c ON c.id = a.ownerid
left join "territory"."sheets_join_territory_join sfdc opp mdr" as d ON a.mdr__c = d.user_id
where b.id is not null
and closedate > '2021-01-01'
and stagename = 'Closed Won'
and d.sub_territory_id is not null



```
