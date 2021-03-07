---
title: Dimensional Table for Booked used for reporting and grouping (Booked)
description: View for getting the connection between grouped cases metrics to the relevant opportunity. Renewals and Amendment cases are included in the grouping.
Status: Interim View
---

```sql
-- CREATE VIEW "SFDC-CASE-W0001-T0002-CASE-ATTRIBUTES" AS
 
 
 WITH OpportunityAttributes AS (
        select
        id,
        name,
        opportunity_number__c
        from
        salesforce_opportunity
 )
 
 WITH AccountAttributes AS (
        select
        id,
        name
        from
        salesforce_account
 )
 
 
 
 -- Define Case Attributes for booked cases needed For Later Joins
 
 
 
 
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
      inet_safer_synergy__c,
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
      d.name as partnername
      
from
      salesforce_case as a
      left outer join OpportunityAttributes as b ON a.opportunity__c = b.id
      left outer join AccountAttributes as c ON a.accountid = c.id
      left outer join AccountAttributes as d ON a.firstin_partner_account__c = d.id
where
      finance_sub_status__c = 'Booked'
 
 
 


```

## View Name: `"SFDC-CASE-W0001-T0002-CASE-ATTRIBUTES"`

| Column | Description |
| --- | --- |
| `id_h`| Hybrid Opportunity ID |
| `opportunity__c`| Opportunity ID |
| `casenumber`| Case Number of the Top Case |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local for `id_h` |
