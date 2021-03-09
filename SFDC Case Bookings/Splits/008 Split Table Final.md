---
title: Adding final attributes to layer over bookings
View Name: sfdc-w003-t006-splits-final-table
Caution: MUST ensure that finance ALL numbers in the split section add up to 100% of NBV of case (Overlay reps not included).
Note: This view will UNION with bookings final table. It needs to have the same columns.
Status: Final View
---

```sql


select
        
        (x.id):: character varying as casenumber, -- this is the modified key casenumber__S
        float4(x.allocation) as allocation,
        (x.teamname)::varchar as dm__c,
        COALESCE(x.allocation * b.mrrchangelocal,0) as mrrchangelocaloverride,
        COALESCE(x.allocation * b.net_bookings_value__c,0) as nbvlocaloverride,
        COALESCE(x.allocation * b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
        COALESCE(x.allocation * b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
        ('Split Case')::varchar as calculationflag,
        
        
        (b.recordtypeid)::varchar as recordtypeid,
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
 "sfdc-w003-t005-splits-key-value-final" as x
 left outer join "SFDC-CASE-W0001-T0002-CASE-ATTRIBUTES" as b ON x.casenumber = b.casenumber



```

## View Name: `"sfdc-w003-t006-splits-final-table"`


| Column | Description |
| --- | --- |
| `casenumber`| Unique identifier, is actually casenumber__S |
| `allocation`| % value used to derive MRR change, NBV while maintaining contract lenght of main case |
| `dm__c`| Team Name (Split Table Team on SFDC). This does not contain the User ID of the DM in the case. |
| `mrrchangelocaloverride`| Override case metrics |
| `nbvlocaloverride`| Override case metrics |
| `Previous_Monthly_Subscription_Fee__c_override`| Override case metrics |
| `Current_Monthly_Subscription_Fee__c_override`| Override case metrics |
