---
View: '"commissions"."plan-rule-2021-003-t001-gam-international-deals-subscriptions"'
PK: opportunity_number__c
---

```sql
              select distinct
              a.opportunity_number__c,
              a.type,
              a.Solution_Type__c,
              a.Qualification__c,
              a.Deal_Support__c,
              a.Presented_Solution_GAM__c,
              a.NAM__c,
              b.nam__c as gamid,
              c.__sub_territory_id,
              a.closedate,
              CASE
                  when a.presented_solution_gam__c::integer = 1 then 1::float
                  when a.deal_support__c::integer = 1 then 0.5::float
                  when a.Qualification__c::integer = 1 then 0.15::float
                  END as allocation
              from
              sfdc_opportunity a
              left join salesforce_account b on a.accountid = b.id
              left join "territory"."sheets_join_territory_join sfdc case gam" as c ON b.nam__c = c.__nam__c
              where
                (
                a.Qualification__c is true or
                a.Deal_Support__c is true or
                a.Presented_Solution_GAM__c is true
                )
              and
                closedate > '2021-01-01'
              and
                iswon is true
              and
                c.__sub_territory_id is not null
                



```
