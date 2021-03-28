---
View: sfdc-w003v1-t007-fact-split-cases-joins-on-union
---

```

-- Start Joins
select
        (a.id) :: character varying (200) as casenumber,
        a.allocation,
        a.allocation * b.mrrchangelocal as mrrchangelocaloverride,
        a.nbvlocaloverride,
        a.allocation * b.Previous_Monthly_Subscription_Fee__c as Previous_Monthly_Subscription_Fee__c_override,
        a.allocation * b.Current_Monthly_Subscription_Fee__c as Current_Monthly_Subscription_Fee__c_override,
        a.calculationflag,
        (b.recordtypeid)::character varying (200) as recordtypeid,
        (a.teamvalue)::character varying (200) as dm__c,
        b.exchange_rate_to_usd__c,
        b.booked_date__c,
        (b.casecurrency__c)::character varying (200) as casecurrency__c,
        (b.contract__c)::character varying (200) as contract__c,
        COALESCE(b.contract_length__c,0) as contract_length__c,
        COALESCE(b.distributor_commission__c,0) as distributor_commission__c,
        COALESCE(b.spiff_commission__c,0) as spiff_commission__c,
        (b.type)::character varying (200) as type,
        (b.inet_type__c)::character varying (200) as inet_type__c,
        COALESCE(a.inet_now_licenses__c_grouped, b.inet_now_licenses__c,0) as inet_now_licenses__c,
        (b.finance_sub_status__c)::character varying (200) as finance_sub_status__c,
        (b.incentive_program_competitive_takeaway__c)::character varying (200) as incentive_program_competitive_takeaway__c,
        (b.incentive_program_qualification__c)::character varying (200) as incentive_program_qualification__c,
        (b.shipping_outside_of_owners_territory__c)::character varying (200) as shipping_outside_of_owners_territory__c,
        (b.accountid)::character varying (200) as accountid,
        (b.firstin_partner_account__c)::character varying (200) as firstin_partner_account__c,
        b.billing_agent__c as billing_agent__c,
        b.billing_agent__c__text,
        
        CASE when COALESCE ( a.inet_safer_synergy__c_grouped, b.inet_safer_synergy__c__text) = 'true' then true
             else false
             end AS inet_safer_synergy__c,
        
        COALESCE( a.inet_safer_synergy__c_grouped, b.inet_safer_synergy__c__text) AS inet_safer_synergy__c__text,
        (b.nam__c)::character varying (200) as nam__c,
        (b.key_account_manager__c)::character varying (200) as key_account_manager__c,
        (left(b.opportunity__c,15))::character varying (200) as opp_id__c,
        (b.oracle_organization__c)::character varying (200) as oracle_organization__c,
        null :: character varying (200) as team_territory_assignment_1__c,
        coalesce(null :: integer) as team_1_net_booking_value_case_currency__c,
        null :: character varying (200) as team_territory_assignment_2__c,
        coalesce(null :: integer) as team_2_net_booking_value_case_currency__c,
        null :: character varying (200) as team_territory_assignment_3__c,
        coalesce(null :: integer) as team_3_net_booking_value_case_currency__c,
        null :: character varying (200) as team_territory_assignment_4__c,
        coalesce(null :: integer) as team_4_net_booking_value_case_currency__c,
        null :: character varying (200) as team_territory_assignment_5__c,
        coalesce(null :: integer) as team_5_net_booking_value_case_currency__c,
        null:: character varying (200) as team_territory_assignment_6__c,
        coalesce(null :: integer) as team_6_net_booking_value_case_currency__c,
        null:: character varying (200) as team_territory_assignment_7__c,
        coalesce(null :: integer) as team_7_net_booking_value_case_currency__c,
        null:: character varying (200) as team_territory_assignment_8__c,
        coalesce(null :: integer) as team_8_net_booking_value_case_currency__c,
        (b.commission_processing_flag__c)::varchar as commission_processing_flag__c,
        (b.oracle_order_number__c):: character varying (200) as oracle_order_number__c,
        (b.oracle_system_number__c):: character varying (200) as oracle_system_number__c,
        (b.opportunityname):: character varying (200) as opportunityname,
        (b.opportunity_number):: character varying (200) as opportunity_number,
        (b.accountname):: character varying (200) as accountname,
        (b.partnername):: character varying (200) as partnername,
        (b.Special_Terms_List__c):: character varying (200) Special_Terms_List__c,
        (b.msastatus):: character varying (200) as msastatus,
        (b.msaenddate) as msaenddate,
        (b.msastartdate) as msastartdate,
        (b.msaownerid):: character varying (200) as msaownerid,
        (b.msaownername):: character varying (200) as msaownername,
        (b.msaname):: character varying (200) as msaname,
        (b.msanumber):: character varying (200) as msanumber
 
from
 "sfdc-w003v1-t006-fact-split-cases-union" as a
 inner join "sfdc-case-w0001v1-t0002-case-attributes" as b ON a.casenumber = b.casenumber
