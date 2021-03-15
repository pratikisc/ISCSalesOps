---
title: Dimensional Table for Booked Cases used for reporting and grouping (Booked)
description: View for getting the connection between grouped cases metrics to the relevant opportunity. Renewals and Amendment cases are included in the grouping.
View: SFDC-CASE-W0001v1-T0002-CASE-ATTRIBUTES
Status: Interim View
---

```sql
-- CREATE VIEW "SFDC-CASE-W0001-T0002-CASE-ATTRIBUTES" AS
  
 
WITH
    dim_opportunity AS (
        select distinct
        id,
        name,
        opportunity_number__c
        from
        sfdc_opportunity
    ),
    dim_account AS (
        select distinct
        id,
        name
        from
        salesforce_account
    ),
   dim_contract AS (
       
       
       select
       a1.id,
       a1.Special_Terms_List__c,
       a1.parent_contract__c,
       a2.status msastatus,
       a2.contract_end_date__c msaenddate,
       a2.startdate msastartdate,
       a2.OwnerId as msaownerid,
       a2.ContractNumber as msanumber,
       a2.name as msaname,
       a3.full_name__c as msaownername
       from salesforce_contract as a1
           left outer join salesforce_contract as a2 on a1.parent_contract__c = a2.id
           left outer join salesforce_user as a3 on a2.ownerid = a3.id
    )
 
 
SELECT
      recordtypeid,
      casenumber,
      dm__c,
      net_bookings_value__c,
      exchange_rate_to_usd__c,
      booked_date__c,
      casecurrency__c,
      contract__c,
      contract_length__c,
      current_monthly_subscription_fee__c,
      previous_monthly_subscription_fee__c,
      current_monthly_subscription_fee__c - previous_monthly_subscription_fee__c as mrrchangelocal,
      distributor_commission__c,
      spiff_commission__c,
      type,
      inet_type__c,
      inet_now_licenses__c,
      finance_sub_status__c,
      incentive_program_competitive_takeaway__c,
      incentive_program_qualification__c,
      shipping_outside_of_owners_territory__c,
      accountid,
      firstin_partner_account__c,
      billing_agent__c,
      case billing_agent__c when true 
            then 'true'
            else 'false'
        end as billing_agent__c__text,
      inet_safer_synergy__c,
      case inet_safer_synergy__c when true 
            then 'true'
            else 'false'
        end as inet_safer_synergy__c__text,
      nam__c,
      key_account_manager__c,
      opportunity__c,
      oracle_organization__c,
      team_territory_assignment_1__c,
      team_1_net_booking_value_case_currency__c,
      team_territory_assignment_2__c,
      team_2_net_booking_value_case_currency__c,
      team_territory_assignment_3__c,
      team_3_net_booking_value_case_currency__c,
      team_territory_assignment_4__c,
      team_4_net_booking_value_case_currency__c,
      team_territory_assignment_5__c,
      team_5_net_booking_value_case_currency__c,
      team_territory_assignment_6__c,
      team_6_net_booking_value_case_currency__c,
      team_territory_assignment_7__c,
      team_7_net_booking_value_case_currency__c,
      team_territory_assignment_8__c,
      team_8_net_booking_value_case_currency__c,
      commission_processing_flag__c,
      oracle_order_number__c,
      oracle_system_number__c,
      b.name as opportunityname,
      b.opportunity_number__c as opportunity_number,
      c.name as accountname,
      d.name as partnername,
      e.Special_Terms_List__c,
      e.msastatus,
      e.msaenddate,
      e.msastartdate,
      e.msaownerid,
      e.msaownername,
      e.msaname,
      e.msanumber
      
from
      salesforce_case as a
      left outer join dim_opportunity as b ON a.opportunity__c = b.id
      left outer join dim_account as c ON a.accountid = c.id
      left outer join dim_account as d ON a.firstin_partner_account__c = d.id
      left outer join dim_contract as e ON a.contract__c = e.id
where
      finance_sub_status__c = 'Booked'
 
 
 


```

## View Name: `"SFDC-CASE-W0001v1-T0002-CASE-ATTRIBUTES"`

| Column | Description |
| --- | --- |
|`All Column Descriptions`| All column descriptions are in the final view  |

