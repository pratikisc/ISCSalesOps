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
        (x.teamname)::character varying as dm__c,
        COALESCE(x.allocation * b.mrrchangelocal,0) as mrrchangelocaloverride,
        COALESCE(x.allocation * b.net_bookings_value__c,0) as nbvlocaloverride,
        COALESCE(x.allocation * b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
        COALESCE(x.allocation * b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
        ('Split Case')::character varying as calculationflag,
        
        
        (b.recordtypeid)::character varying as recordtypeid,
        b.exchange_rate_to_usd__c,
        b.booked_date__c,
        (b.casecurrency__c)::character varying as casecurrency__c,
        (b.contract__c)::character varying as contract__c,
        COALESCE(b.contract_length__c,0) as contract_length__c,
        COALESCE(b.distributor_commission__c,0) as distributor_commission__c,
        COALESCE(b.spiff_commission__c,0) as spiff_commission__c,
        (b.type)::character varying as type,
        (b.inet_type__c)::character varying as inet_type__c,
        COALESCE(b.inet_now_licenses__c,0) as inet_now_licenses__c,
        (b.finance_sub_status__c)::character varying as finance_sub_status__c,
        (b.incentive_program_competitive_takeaway__c)::character varying as incentive_program_competitive_takeaway__c,
        (b.incentive_program_qualification__c)::character varying as incentive_program_qualification__c,
        (b.shipping_outside_of_owners_territory__c)::character varying as shipping_outside_of_owners_territory__c,
        (b.accountid)::character varying as accountid,
        (b.firstin_partner_account__c)::character varying as firstin_partner_account__c,
        b.billing_agent__c as billing_agent__c,
        b.inet_safer_synergy__c as inet_safer_synergy__c,
        (b.nam__c)::character varying as nam__c,
        (b.key_account_manager__c)::character varying as key_account_manager__c,
        (left(b.opportunity__c,15))::character varying as opp_id__c,
        (b.oracle_organization__c)::character varying as oracle_organization__c,
        null :: character varying as team_territory_assignment_1__c,
        coalesce(null :: integer) as team_1_net_booking_value_case_currency__c,
        null :: character varying as team_territory_assignment_2__c,
        coalesce(null :: integer) as team_2_net_booking_value_case_currency__c,
        null :: character varying as team_territory_assignment_3__c,
        coalesce(null :: integer) as team_3_net_booking_value_case_currency__c,
        null :: character varying as team_territory_assignment_4__c,
        coalesce(null :: integer) as team_4_net_booking_value_case_currency__c,
        null :: character varying as team_territory_assignment_5__c,
        coalesce(null :: integer) as team_5_net_booking_value_case_currency__c,
        null:: character varying as team_territory_assignment_6__c,
        coalesce(null :: integer) as team_6_net_booking_value_case_currency__c,
        null:: character varying as team_territory_assignment_7__c,
        coalesce(null :: integer) as team_7_net_booking_value_case_currency__c,
        null:: character varying as team_territory_assignment_8__c,
        coalesce(null :: integer) as team_8_net_booking_value_case_currency__c,
        (commission_processing_flag__c)::varchar as commission_processing_flag__c,
        (b.oracle_order_number__c):: character varying as oracle_order_number__c,
        (b.oracle_system_number__c):: character varying as oracle_system_number__c,
        (b.opportunityname):: character varying as opportunityname,
        (b.opportunity_number):: character varying as opportunity_number,
        (b.accountname):: character varying as accountname,
        (b.partnername):: character varying as partnername,
        (b.Special_Terms_List__c):: character varying Special_Terms_List__c,
        (b.msastatus):: character varying as msastatus,
        (b.msaenddate) as msaenddate,
        (b.msastartdate) as msastartdate,
        (b.msaownerid):: character varying as msaownerid,
        (b.msaownername):: character varying as msaownername,
        (b.msaname):: character varying as msaname,
        (b.msanumber):: character varying as msanumber
 
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
