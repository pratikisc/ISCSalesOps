---
View: sfdc-case-w0001-t0003-a-final case numbers
Note: Splits removed again from other joins
---


```sql

  
(
    SELECT
        casenumber :: character varying (200) as casenumber
    FROM
        "SFDC-CASE-W0001-T0001-GROUPED-CASES" -- Grouped Renewal and Amendment cases by Opportunity

)
UNION
(
    SELECT
    casenumber :: character varying (200) as casenumber
    FROM
    salesforce_case
    WHERE
        (
            type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and
            finance_sub_status__c = 'Booked'
        )
        or
        (
              type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and
              finance_sub_status__c = 'Booked' and
              opportunity__c is null
        )

)
