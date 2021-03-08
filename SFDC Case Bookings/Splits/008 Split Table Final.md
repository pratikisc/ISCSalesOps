---
title: Adding final attributes to layer over bookings
View Name: sfdc-w003-t006-splits-final-table
Note: This view will UNION with bookings final table. It needs to have the same columns.
Status: Final View
---

```sql


select
        x.casenumber,
        x.id as casenumber__s,
        x.allocation,
        COALESCE(x.allocation * b.mrrchangelocal,0) as mrrchangelocaloverride,
        COALESCE(x.allocation * b.net_bookings_value__c,0) as nbvlocaloverride,
        COALESCE(x.allocation * b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
        COALESCE(x.allocation * b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
        'Split Case' as calculationflag,
        b.recordtypeid,
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
        b.commission_processing_flag__c,
        b.oracle_order_number__c,
        b.oracle_system_number__c,
        b.opportunityname,
        b.opportunity_number,
        b.accountname,
        b.partnername,
        b.Special_Terms_List__c,
        b.msastatus,
        b.msaenddate,
        b.msastartdate,
        b.msaownerid,
        b.msaownername
 
from
 "sfdc-w003-t005-splits-key-value-final" as x
 left outer join "SFDC-CASE-W0001-T0001-GROUPED-CASES" as a ON x.casenumber = a.casenumber



```

## View Name: `"sfdc-w003-t006-splits-final-table"`


| Column | Description |
| --- | --- |
| `casenumber__s`| Unique identifier |
| `allocation`| % value used to derive MRR change, NBV while maintaining contract lenght of main case |
| `mrrchangelocaloverride`| Override case metrics |
| `nbvlocaloverride`| Override case metrics |
| `Previous_Monthly_Subscription_Fee__c_override`| Override case metrics |
| `Current_Monthly_Subscription_Fee__c_override`| Override case metrics |
