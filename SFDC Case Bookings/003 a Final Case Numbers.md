---
View: sfdc-case-w0001-t0003-a-final case numbers
Note: Splits removed again from other joins
---


```sql

WITH caselist AS (   
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
 )
 

 -- Start Joins
select
        (x.casenumber) :: character varying (200) as casenumber
 
from
caselist as x
left outer join "SFDC-CASE-W0001-T0001-GROUPED-CASES" as a ON x.casenumber = a.casenumber

-- left anti join for cases already in split

left join "sfdc-w003-t005-splits-key-value-final" as j1 ON x.casenumber = j1.casenumber
where
j1.casenumber is null
