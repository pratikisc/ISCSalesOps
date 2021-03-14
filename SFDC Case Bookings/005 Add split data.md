---
View: sfdc-case-w0001-t0005-final-joined-view-with-splits
Use: Territory Bookings in CIQ
---

```

with split_non_grouped AS (
                    select distinct
                    id,
                    casenumber,
                    teamname,
                    allocation,
                    calculationflag
                    from
                    "sfdc-w003-t005-final-metrics-to-join"
                    WHERE
                      -- Remember to check this same list in the WITH ( CaseList ) table in Final View. This is the inverse of Case List.
                        (
                            type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and
                            finance_sub_status__c = 'Booked' and
                            casenumber is not null
                        )
                        or
                        (
                              type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and
                              finance_sub_status__c = 'Booked' and
                              id_h is null and
                              casenumber is not null
                        )
),

splits_grouped AS (
                    select distinct
                    id_h,
                    id,
                    casenumber,
                    teamname,
                    allocation,
                    calculationflag
                    from
                    "sfdc-w003-t005-final-metrics-to-join"
                    WHERE
                      -- Remember to check this same list in the WITH ( CaseList ) table in Final View
                      type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and    
                      finance_sub_status__c = 'Booked' and
                      id_h is not null
  

)

select
        
        a.id_h,
        a.casenumber,
        a.allocation,
        coalesce( b.allocation*a.mrrchangelocaloverride, c.allocation* a.mrrchangelocaloverride, a.mrrchangelocaloverride) as mrrchangelocaloverride,
        coalesce( b.allocation*a.nbvlocaloverride, c.allocation*a.nbvlocaloverride, a.nbvlocaloverride) as nbvlocaloverride,
        coalesce( b.allocation*a.Previous_Monthly_Subscription_Fee__c_override, c.allocation*a.Previous_Monthly_Subscription_Fee__c_override,a.Previous_Monthly_Subscription_Fee__c_override) as Previous_Monthly_Subscription_Fee__c_override,
        
        coalesce( b.allocation*a.current_Monthly_Subscription_Fee__c_override, c.allocation*a.current_Monthly_Subscription_Fee__c_override,a.current_Monthly_Subscription_Fee__c_override) as current_Monthly_Subscription_Fee__c_override,
        coalesce ( b.teamname, c.teamname, dm__c) as dm__c__ciq,
        coalesce ( b.teamname, c.teamname) as teamname,
        coalesce ( b.calculationflag, c.calculationflag, a.calculationflag) as calculationflag,
        a.recordtypeid,
        a.dm__c,
        a.exchange_rate_to_usd__c,
        a.booked_date__c,
        a.casecurrency__c,
        a.contract__c,
        a.contract_length__c,
        a.distributor_commission__c,
        a.spiff_commission__c,
        a.type,
        a.inet_type__c,
        a.inet_now_licenses__c,
        a.finance_sub_status__c,
        a.incentive_program_competitive_takeaway__c,
        a.incentive_program_qualification__c,
        a.shipping_outside_of_owners_territory__c,
        a.accountid,
        a.firstin_partner_account__c,
        a.billing_agent__c,
        a.inet_safer_synergy__c,
        a.nam__c,
        a.key_account_manager__c,
        a.opp_id__c,
        a.oracle_organization__c,
        a.team_territory_assignment_1__c,
        a.team_1_net_booking_value_case_currency__c,
        a.team_territory_assignment_2__c,
        a.team_2_net_booking_value_case_currency__c,
        a.team_territory_assignment_3__c,
        a.team_3_net_booking_value_case_currency__c,
        a.team_territory_assignment_4__c,
        a.team_4_net_booking_value_case_currency__c,
        a.team_territory_assignment_5__c,
        a.team_5_net_booking_value_case_currency__c,
        a.team_territory_assignment_6__c,
        a.team_6_net_booking_value_case_currency__c,
        a.team_territory_assignment_7__c,
        a.team_7_net_booking_value_case_currency__c,
        a.team_territory_assignment_8__c,
        a.team_8_net_booking_value_case_currency__c,
        a.commission_processing_flag__c,
        a.oracle_order_number__c,
        a.oracle_system_number__c,
        a.opportunityname,
        a.opportunity_number,
        a.accountname,
        a.partnername,
        a.Special_Terms_List__c,
        a.msastatus,
        a.msaenddate,
        a.msastartdate,
        a.msaownerid,
        a.msaownername,
        a.msaname,
        a.msanumber
from
        "sfdc-case-w0001-t0003-final-joined-view" as a
        left join splits_grouped as b on a.id_h = b.id_h
        left join split_non_grouped as c on a.casenumber = c.casenumber
