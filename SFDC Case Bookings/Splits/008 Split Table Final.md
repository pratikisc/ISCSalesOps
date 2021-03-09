---
title: Adding final attributes to layer over bookings
View Name: sfdc-w003-t006-splits-final-table
Caution: MUST ensure that finance ALL numbers in the split section add up to 100% of NBV of case (Overlay reps not included).
Note: This view will UNION with bookings final table. It needs to have the same columns.
Status: Final View
---

```sql


select
        
        text(x.id) as casenumber, -- this is the modified key casenumber__S
        float4(x.allocation),
        text(x.teamname) as dm__c,
        COALESCE(x.allocation * b.mrrchangelocal,0) as mrrchangelocaloverride,
        COALESCE(x.allocation * b.net_bookings_value__c,0) as nbvlocaloverride,
        COALESCE(x.allocation * b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
        COALESCE(x.allocation * b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
        text('Split Case') as calculationflag,
        
        
        float4 (1) as allocation,
        text(b.recordtypeid) as recordtypeid,
        text(b.exchange_rate_to_usd__c) as exchange_rate_to_usd__c,
        b.booked_date__c,
        text(b.casecurrency__c) as casecurrency__c,
        text(b.contract__c) as contract__c,
        COALESCE(b.contract_length__c,0) as contract_length__c,
        COALESCE(b.distributor_commission__c,0) as distributor_commission__c,
        COALESCE(b.spiff_commission__c,0) as spiff_commission__c,
        text(b.type) as type,
        text(b.inet_type__c) as inet_type__c,
        COALESCE(b.inet_now_licenses__c,0) as inet_now_licenses__c,
        text(b.finance_sub_status__c) as finance_sub_status__c,
        text(b.incentive_program_competitive_takeaway__c) as incentive_program_competitive_takeaway__c,
        text(b.incentive_program_qualification__c) as incentive_program_qualification__c,
        text(b.shipping_outside_of_owners_territory__c) as shipping_outside_of_owners_territory__c,
        text(b.accountid) as accountid,
        text(b.firstin_partner_account__c) as firstin_partner_account__c,
        b.billing_agent__c as billing_agent__c,
        b.inet_safer_synergy__c as inet_safer_synergy__c,
        text(b.nam__c) as nam__c,
        text(b.key_account_manager__c) as key_account_manager__c,
        text(left(b.opportunity__c,15)) as opp_id__c,
        text(b.oracle_organization__c) as oracle_organization__c,
        null as team_territory_assignment_1__c,
        null as team_1_net_booking_value_case_currency__c,
        null as team_territory_assignment_2__c,
        null as team_2_net_booking_value_case_currency__c,
        null as team_territory_assignment_3__c,
        null as team_3_net_booking_value_case_currency__c,
        null as team_territory_assignment_4__c,
        null as team_4_net_booking_value_case_currency__c,
        null as team_territory_assignment_5__c,
        null as team_5_net_booking_value_case_currency__c,
        null as team_territory_assignment_6__c,
        null as team_6_net_booking_value_case_currency__c,
        null as team_territory_assignment_7__c,
        null as team_7_net_booking_value_case_currency__c,
        null as team_territory_assignment_8__c,
        null as team_8_net_booking_value_case_currency__c,
        text(commission_processing_flag__c) as commission_processing_flag__c,
        text(b.oracle_order_number__c) as oracle_order_number__c,
        text(b.oracle_system_number__c) as oracle_system_number__c,
        text(b.opportunityname) as opportunityname,
        text(b.opportunity_number) as opportunity_number,
        text(b.accountname) as accountname,
        text(b.partnername) as partnername,
        text(b.Special_Terms_List__c) Special_Terms_List__c,
        text(b.msastatus) as msastatus,
        date(b.msaenddate) as msaenddate,
        text(b.msastartdate) as msastartdate,
        text(b.msaownerid) as msaownerid,
        text(b.msaownername) as msaownername,
        text(b.msaname) as msaname,
        text(b.msanumber) as msanumber
 
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
