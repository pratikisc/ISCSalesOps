---
title: Final View for CIQ / Power BI Sales Team
description: Final view to calculate Subscription Bookings
Status: Final View

---

```sql
-- CREATE VIEW "SFDC-CASE-W0001-T0003-FINAL-VIEW" AS


-- UNION to get relevant case numbers
WITH caselist AS (   
        (
            SELECT
                casenumber
            FROM
                "SFDC-CASE-W0001-T0001-GROUPED-CASES" -- Grouped Renewal and Amendment cases by Opportunity
    
        )
        UNION
        (
            SELECT
            casenumber
            FROM
            salesforce_case
            WHERE
                (
                    type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and
                    finance_sub_status__c = 'Booked'
                )
                or
                (
                      type in ('Renewal', 'Amendment') and
                      finance_sub_status__c = 'Booked' and
                      opportunity__c is null
                )
    
        )
 )
 

 -- Start Joins
select
        x.casenumber,
        a.nbv_local_grouped,
        a.Previous_Monthly_Subscription_Fee__c_grouped	,
        a.Current_Monthly_Subscription_Fee__c_grouped,
        a.mrr_change_local_grouped,
        COALESCE(a.mrr_change_local_grouped, b.mrrchangelocal,0) as mrrchangelocaloverride,
        COALESCE(a.nbv_local_grouped,b.net_bookings_value__c,0) as nbvlocaloverride,
        COALESCE(a.Previous_Monthly_Subscription_Fee__c_grouped,b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
        COALESCE(a.Current_Monthly_Subscription_Fee__c_grouped,b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
        a.calculationflag,
        b.recordtypeid,
        -- b.casenumber,
        b.dm__c,
        b.net_bookings_value__c,
        b.exchange_rate_to_usd__c,
        b.booked_date__c,
        b.casecurrency__c,
        b.contract__c,
        COALESCE(b.contract_length__c,0) as contract_length__c,
        b.current_monthly_subscription_fee__c,
        b.previous_monthly_subscription_fee__c,
        b.current_monthly_subscription_fee__c - previous_monthly_subscription_fee__c as mrrchangelocal,
        b.distributor_commission__c,
        b.spiff_commission__c,
        b.type,
        b.inet_type__c,
        b.inet_now_licenses__c,
        b.finance_sub_status__c,
        b.incentive_program_competitive_takeaway__c,
        b.incentive_program_qualification__c,
        b.shipping_outside_of_owners_territory__c,
        b.accountid,
        b.firstin_partner_account__c,
        b.billing_agent__c,
        b.inet_safer_synergy__c,
        b.nam__c,
        b.key_account_manager__c,
        b.opportunity__c,
        b.oracle_organization__c,
        b.team_territory_assignment_1__c,
        b.team_1_net_booking_value_case_currency__c,
        b.team_territory_assignment_2__c,
        b.team_2_net_booking_value_case_currency__c,
        b.team_territory_assignment_3__c,
        b.team_3_net_booking_value_case_currency__c,
        b.team_territory_assignment_4__c,
        b.team_4_net_booking_value_case_currency__c,
        b.team_territory_assignment_5__c,
        b.team_5_net_booking_value_case_currency__c,
        b.team_territory_assignment_6__c,
        b.team_6_net_booking_value_case_currency__c,
        b.team_territory_assignment_7__c,
        b.team_7_net_booking_value_case_currency__c,
        b.team_territory_assignment_8__c,
        b.team_8_net_booking_value_case_currency__c,
        b.commission_processing_flag__c,
        b.oracle_order_number__c,
        b.oracle_system_number__c,
        b.opportunityname,
        b.opportunity_number,
        b.accountname,
        b.partnername
 
from
 caselist as x
 left outer join "SFDC-CASE-W0001-T0001-GROUPED-CASES" as a ON x.casenumber = a.casenumber
 inner join "sfdc-case-w0001-t0002-case-attributes" as b ON x.casenumber = b.casenumber
 
 
 -- Use later.. COALESCE(b.net_bookings_value__c_override, net_bookings_value__c) AS 

```

## View Name: `"SFDC-CASE-W0001-T0003-FINAL-VIEW"`


| Column | Description |
| --- | --- |
| `id_h`| Hybrid Opportunity ID |
| `opportunity__c`| Opportunity ID |
| `casenumber`| Case Number of the Top Case |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local for `id_h` |
