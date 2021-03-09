---
title: Final View for CIQ / Power BI Sales Team
description: Final view to calculate Subscription Bookings. Cases (Ungrouped cases in prev. step) with Splits are also removed using left anti join. All Team values are replaced with null
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
        (x.casenumber) :: character varying as casenumber,
        float4 (1) as allocation,
        COALESCE(a.mrr_change_local_grouped, b.mrrchangelocal,0) as mrrchangelocaloverride,
        COALESCE(a.nbv_local_grouped,b.net_bookings_value__c,0) as nbvlocaloverride,
        COALESCE(a.Previous_Monthly_Subscription_Fee__c_grouped,b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
        COALESCE(a.Current_Monthly_Subscription_Fee__c_grouped,b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
        (coalesce(a.calculationflag,'Case Value'))::varchar as calculationflag,
        (b.recordtypeid)::varchar as recordtypeid,
        (b.dm__c)::varchar as dm__c,
        b.exchange_rate_to_usd__c,
        b.booked_date__c,
        (b.casecurrency__c)::varchar as casecurrency__c,
        (b.contract__c)::varchar as contract__c,
        COALESCE(b.contract_length__c,0) as contract_length__c,
        COALESCE(b.distributor_commission__c,0) as distributor_commission__c,
        COALESCE(b.spiff_commission__c,0) as spiff_commission__c,
        (b.type)::varchar as type,
        (b.inet_type__c)::varchar as inet_type__c,
        COALESCE(b.inet_now_licenses__c,0) as inet_now_licenses__c,
        (b.finance_sub_status__c)::varchar as finance_sub_status__c,
        (b.incentive_program_competitive_takeaway__c)::varchar as incentive_program_competitive_takeaway__c,
        (b.incentive_program_qualification__c)::varchar as incentive_program_qualification__c,
        (b.shipping_outside_of_owners_territory__c)::varchar as shipping_outside_of_owners_territory__c,
        (b.accountid)::varchar as accountid,
        (b.firstin_partner_account__c)::varchar as firstin_partner_account__c,
        b.billing_agent__c as billing_agent__c,
        b.inet_safer_synergy__c as inet_safer_synergy__c,
        (b.nam__c)::varchar as nam__c,
        (b.key_account_manager__c)::varchar as key_account_manager__c,
        (left(b.opportunity__c,15))::varchar as opp_id__c,
        (b.oracle_organization__c)::varchar as oracle_organization__c,
        null as team_territory_assignment_1__c,
        coalesce(null) as team_1_net_booking_value_case_currency__c,
        null as team_territory_assignment_2__c,
        coalesce(null) as team_2_net_booking_value_case_currency__c,
        null as team_territory_assignment_3__c,
        coalesce(null) as team_3_net_booking_value_case_currency__c,
        null as team_territory_assignment_4__c,
        coalesce(null) as team_4_net_booking_value_case_currency__c,
        null as team_territory_assignment_5__c,
        coalesce(null) as team_5_net_booking_value_case_currency__c,
        null as team_territory_assignment_6__c,
        coalesce(null) as team_6_net_booking_value_case_currency__c,
        null as team_territory_assignment_7__c,
        coalesce(null) as team_7_net_booking_value_case_currency__c,
        null as team_territory_assignment_8__c,
        coalesce(null) as team_8_net_booking_value_case_currency__c,
        (commission_processing_flag__c)::varchar as commission_processing_flag__c,
        (b.oracle_order_number__c)::varchar as oracle_order_number__c,
        (b.oracle_system_number__c)::varchar as oracle_system_number__c,
        (b.opportunityname)::varchar as opportunityname,
        (b.opportunity_number)::varchar as opportunity_number,
        (b.accountname)::varchar as accountname,
        (b.partnername)::varchar as partnername,
        (b.Special_Terms_List__c)::varchar Special_Terms_List__c,
        (b.msastatus)::varchar as msastatus,
        (b.msaenddate) as msaenddate,
        (b.msastartdate) as msastartdate,
        (b.msaownerid)::varchar as msaownerid,
        (b.msaownername)::varchar as msaownername,
        (b.msaname)::varchar as msaname,
        (b.msanumber)::varchar as msanumber
 
from
 caselist as x
 left outer join "SFDC-CASE-W0001-T0001-GROUPED-CASES" as a ON x.casenumber = a.casenumber
 inner join "sfdc-case-w0001-t0002-case-attributes" as b ON x.casenumber = b.casenumber

-- !!!! Applying Left Anti Join to only include / group cases that are not present in the Split Table. i.e. if there is split, then it is excluded See: https://mode.com/blog/anti-join-examples/

left join "sfdc-w003-t005-splits-key-value-final" as j1 ON x.casenumber = j1.casenumber
where
j1.casenumber is null
 

```

## View Name: `"SFDC-CASE-W0001-T0003-FINAL-VIEW"`


| Column | Description |
| --- | --- |
| `id_h`| Hybrid Opportunity ID |
| `opportunity__c`| Opportunity ID |
| `casenumber`| Case Number of the Top Case if grouped by opportunity, else standalone `casenumber` |
| `nbvlocaloverride` | NBV Local override value, grouped by `id_h` |
| `calculationflag` | To know if case calculation was grouped by Opportunity `id_h` |
| `mrrchangelocaloverride` | MRR Change Local override value, grouped by `id_h` |
| `Previous_Monthly_Subscription_Fee__c_override`| Previous Monthly Local Override value, grouped by `id_h` |
| `Current_Monthly_Subscription_Fee__c_override` | Current Monthly Local Override value, grouped by `id_h` |
|`recordtypeid`|  |
|`dm__c` |  |
|`net_bookings_value__c` |  |
|`exchange_rate_to_usd__c` |  |
|`booked_date__c` |  |
|`casecurrency__c` |  |
|`contract__c` |  |
|`contract_length__c` | COALESCE function used to substitute NULL values with zero |
|`current_monthly_subscription_fee__c` |  |
|`previous_monthly_subscription_fee__c` |  |
|`distributor_commission__c` |  |
|`spiff_commission__c` |  |
|`type` |  |
|`inet_type__c` |  |
|`inet_now_licenses__c` |  |
|`finance_sub_status__c` |  |
|`incentive_program_competitive_takeaway__c` | 2020 Program: No SPIFF programs in 2021 |
|`incentive_program_qualification__c` |  2020 Program: No SPIFF programs in 2021 |
|`shipping_outside_of_owners_territory__c` |  |
|`accountid` |  |
|`firstin_partner_account__c` |  |
|`billing_agent__c` |  |
|`inet_safer_synergy__c` |  |
|`nam__c` |  |
|`key_account_manager__c` |  |
|`opp_id__c` | left 15 chars taken to match opp_id__c |
|`oracle_organization__c` |  |
|`team_territory_assignment_1__c` | null |
|`team_1_net_booking_value_case_currency__c` | null |
|`team_territory_assignment_2__c` | null |
|`team_2_net_booking_value_case_currency__c` |null  |
|`team_territory_assignment_3__c` | null |
|`team_3_net_booking_value_case_currency__c` | null |
|`team_territory_assignment_4__c` | null |
|`team_4_net_booking_value_case_currency__c` | null |
|`team_territory_assignment_5__c` | null |
|`team_5_net_booking_value_case_currency__c` | null |
|`team_territory_assignment_6__c` | null|
|`team_6_net_booking_value_case_currency__c` | null |
|`team_territory_assignment_7__c` | null |
|`team_7_net_booking_value_case_currency__c` | null |
|`team_territory_assignment_8__c` | null |
|`team_8_net_booking_value_case_currency__c` | null |
|`commission_processing_flag__c` | Used by finance to flag cases that are not assigned any negative credit Sales Reps. If there is a picklist value selected in this field, then it is excluded from grouping calculations at an earlier part of the calculation. If there are cases without opportunities AND `commission_processing_flag__c` has a value, then it will need to be excluded |
|`oracle_order_number__c` | Used to Track 'iNet' Synergy orders for SAFER Team. Process not yet defined |
|`oracle_system_number__c` |  |
|`opportunityname` | Useful in statement design Group By |
|`opportunity_number` | Opp#xxx useful for look up in statement design |
|`accountname` | Useful in statement design Group By |
|`partnername` | Useful in statement design Group By  |
|`Special_Terms_List__c` | Used for Accelerators, value = Auto Renew contracts |
|`MSAOwnerId` | MSA items used to determine automatic overlay policies per Governing Policy |
|`MSAStatus` | MSA has to be active |
|`MSAEndDate` | MSA End Date must be after booked date |
|`MSAStartDate` |MSA Start Date must be before booked date |
|`MSAOwnerName` | MSA Owner gets credit per Governing Policy |
|`msaname`| Name of Master Service Agreement|
|`msanumber`| Contract Number of MSA |
