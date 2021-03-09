

```sql
SFDC-CASE-W0001-T0004-FINAL-UNION-BASE-AND-SPLIT


-- base booking table

WITH A AS (
        select
        casenumber,
        allocation,
        mrrchangelocaloverride,
        nbvlocaloverride,
        Previous_Monthly_Subscription_Fee__c_override,
        Current_Monthly_Subscription_Fee__c_override,
        calculationflag,
        recordtypeid,
        dm__c,
        exchange_rate_to_usd__c,
        booked_date__c,
        casecurrency__c,
        contract__c,
        contract_length__c,
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
        opp_id__c,
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
        opportunityname,
        opportunity_number,
        accountname,
        partnername,
        Special_Terms_List__c,
        msastatus,
        msaenddate,
        msastartdate,
        msaownerid,
        msaownername,
        msaname,
        msanumber
      from
      "SFDC-CASE-W0001-T0003-FINAL-VIEW"
),
        B AS (
  
-- split table

select
        casenumber,
        allocation,
        mrrchangelocaloverride,
        nbvlocaloverride,
        Previous_Monthly_Subscription_Fee__c_override,
        Current_Monthly_Subscription_Fee__c_override,
        calculationflag,
        recordtypeid,
        dm__c,
        exchange_rate_to_usd__c,
        booked_date__c,
        casecurrency__c,
        contract__c,
        contract_length__c,
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
        opp_id__c,
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
        opportunityname,
        opportunity_number,
        accountname,
        partnername,
        Special_Terms_List__c,
        msastatus,
        msaenddate,
        msastartdate,
        msaownerid,
        msaownername,
        msaname,
        msanumber
      from
      "sfdc-w003-t006-splits-final-table"
  )
  
  SELECT
  casenumber
  from
  *
  UNION
  select
  *
  from
  b


   
