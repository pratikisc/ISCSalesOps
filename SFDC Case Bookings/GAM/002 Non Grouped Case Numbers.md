---
View: '"commissions"."sfdc-case-w0001v2-t0002-non-grouped-cases"'
Note: All case numbers from ungrouped; Commission Processing Flag = NULL;
PK: casenumber
---


```sql

  
    SELECT
    casenumber,
    'Case Value' as calculationflag,
    1 as allocation
    FROM
    salesforce_case
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
