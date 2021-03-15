---
View: sfdc-case-w0001-t0003-a-final case numbers
Note: All the case numbers from grouped cases + case numbers from ungrouped; Commission Processing Flag = NULL; Splits excluded here.
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
            type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
            finance_sub_status__c = 'Booked' and
            Commission_Processing_Flag__c is null
            
            -- !!!! Removing Splits from Grouping here
		        and a.team_territory_assignment_1__c is null
        )
        or
        (
              type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
              finance_sub_status__c = 'Booked' and
              opportunity__c is null and
              Commission_Processing_Flag__c is null
              
              -- !!!! Removing Splits from Grouping here
		          and a.team_territory_assignment_1__c is null
        )

)
