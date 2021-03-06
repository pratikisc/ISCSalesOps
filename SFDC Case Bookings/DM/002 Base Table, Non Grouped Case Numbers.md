---
View: '"commissions"."sfdc-case-w0002-dm-t0002-non-grouped-cases"'
Note: All case numbers from ungrouped; Commission Processing Flag = NULL
PK: split_id
---

Notes
- MRR Change override applied here

```sql

  
  SELECT
    a.casenumber,
    split_id,
    'Case Value' as calculationflag,
    b.dm_sub_territory_incl_split,
    b.teamvalue,
    b.allocation,
    coalesce(b.allocation* a.net_bookings_value__c,0) as net_bookings_value__c,
    a.Contract_Length__c,
    coalesce(b.allocation* a.Current_Monthly_Subscription_Fee__c,0) as current_monthly_subscription_fee__c,
    coalesce(b.allocation* a.Previous_Monthly_Subscription_Fee__c,0) as previous_monthly_subscription_fee__c,
    
    -- !!! MRR Change Override applied here
    
    coalesce( b.allocation * c.override_mrrchangelocal,
              coalesce(b.allocation* a.Current_Monthly_Subscription_Fee__c,0) - coalesce(b.allocation* a.Previous_Monthly_Subscription_Fee__c,0)
            )
            as MRRChangeLocal,
   	coalesce(b.allocation* a.inet_now_licenses__c,0) as inet_now_licenses__c

    FROM
     "commissions"."reference-sfdc-case-dm-subterritory-incl-splits" as b
     left join salesforce_case a ON a.casenumber = b.casenumber
     left join "commissions"."reference-sfdc-case-details-override-v2" as c ON b.casenumber = c.casenumber

    WHERE
        (
           type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
           finance_sub_status__c = 'Booked' and
           Commission_Processing_Flag__c is null
         )
         or
        (
            type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
            finance_sub_status__c = 'Booked' and
            opportunity__c is null and
            Commission_Processing_Flag__c is null
        )
        
  ```
