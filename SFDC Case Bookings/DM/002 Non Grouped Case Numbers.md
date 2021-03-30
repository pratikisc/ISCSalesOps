---
View: '"commissions"."sfdc-case-w0002-dm-t0002-non-grouped-cases"'
Note: All case numbers from ungrouped; Commission Processing Flag = NULL;
---


```sql

  
  SELECT
    a.casenumber,
    'Case Value' as calculationflag,
    b.dm_sub_territory_incl_split,
    1 as allocation
    FROM
    salesforce_case a
    left join "commissions"."reference-sfdc-case-dm-subterritory-incl-splits" as b ON a.casenumber = b.casenumber
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
