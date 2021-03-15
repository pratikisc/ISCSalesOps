---
View: sfdc-case-w0001-t0005-final-joined-view-with-splits
Notes: Calculation Flag, dm__c__original
Use: Territory Bookings in CIQ
---

```


!!!! Check Up: Do an inner join between split_non_grouped and splits_grouped to ensure consistent unique foreign key.

with split_non_grouped AS (
                    select distinct
                    id,
                    casenumber,
                    teamname,
                    bookingvalue,
                    'Split / Case Value' as calculationflag
                    from
                    "sfdc-w003-t005-final-metrics-to-join"
                    WHERE
                      -- Remember to check this same list in the WITH ( CaseList ) table in `003 a Final Case Numbers.md`. This is the inverse of Case List.
                        (
                            type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
                            finance_sub_status__c = 'Booked'
                        )
                        or
                        (
                              type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
                              finance_sub_status__c = 'Booked' and
                              id_h is null
                        )
),

splits_grouped AS (
                    select distinct
                    id_h,
                    id,
                    casenumber,
                    teamname,
                    bookingvalue,
                    'Split / Grouped Value' as calculationflag
                    from
                    "sfdc-w003-t005-final-metrics-to-join"
                    WHERE
                      -- Remember to check this same list in the WITH ( CaseList ) table in Final View
                      type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and    
                      finance_sub_status__c = 'Booked' and
                      id_h is not null
  

)

select
        
        a.id_h,
        coalesce( b.id, c.id, a.casenumber) as casenumber, -- overriding the Case Number__s for Split Cases
        a.casenumber as casenumber_original,
        coalesce ( b.bookingvalue/a.nbvlocaloverride, c.bookingvalue/a.nbvlocaloverride, a.allocation) as allocation,
        coalesce( b.bookingvalue/a.nbvlocaloverride*a.mrrchangelocaloverride, c.bookingvalue/a.nbvlocaloverride* a.mrrchangelocaloverride, a.mrrchangelocaloverride) as mrrchangelocaloverride,
        coalesce( b.bookingvalue, c.bookingvalue, a.nbvlocaloverride) as nbvlocaloverride,
        coalesce( b.bookingvalue/a.nbvlocaloverride*a.Previous_Monthly_Subscription_Fee__c_override, c.bookingvalue/a.nbvlocaloverride*a.Previous_Monthly_Subscription_Fee__c_override,a.Previous_Monthly_Subscription_Fee__c_override) as Previous_Monthly_Subscription_Fee__c_override,
        
        coalesce( b.bookingvalue/a.nbvlocaloverride*a.current_Monthly_Subscription_Fee__c_override, c.bookingvalue/a.nbvlocaloverride*a.current_Monthly_Subscription_Fee__c_override,a.current_Monthly_Subscription_Fee__c_override) as current_Monthly_Subscription_Fee__c_override,
        coalesce ( b.teamname, c.teamname, a.dm__c) as dm__c,
        a.dm__c as dm__c__original,
        coalesce ( b.teamname, c.teamname) as teamname,
        coalesce ( b.calculationflag, c.calculationflag, a.calculationflag) as calculationflag,
        a.recordtypeid,
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
        a.billing_agent__c__text,
        a.inet_safer_synergy__c,
        a.inet_safer_synergy__c__text,
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
        left join splits_grouped as b on a.id_h = b.id_h   -- !!!! Ensure proper foreign key
        left join split_non_grouped as c on a.casenumber = c.casenumber

where
        a.nbvlocaloverride <> 0
