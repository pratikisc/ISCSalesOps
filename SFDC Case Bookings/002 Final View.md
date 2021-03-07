---
title: Unique List of Case Numbers (Booked)
description: View for getting the connection between grouped cases metrics to the relevant opportunity. Renewals and Amendment cases are included in the grouping.
Status: Final View
usage: This is an interim view for downstream calculations on Salesforce booking can be used in many different salesforce reports for a quick connection between `accountid` and the relevant `email`.
---

```sql
-- CREATE VIEW ___ AS


-- UNION to get relevant case numbers
WITH caselist AS (   
        (
            SELECT
                casenumber
            FROM
                "SFDC-CASE-W0001-T0001-GROUPED-CASES"
    
        )
        UNION
        (
            SELECT
            casenumber
            FROM
            salesforce_case
            WHERE
                (
                    type not in ('Renewal', 'Amendment') and
                    net_bookings_value__c <> 0 and
                    finance_sub_status__c = 'Booked' and
                    opportunity__c is null
                )
                or
                (
                      type in ('Renewal', 'Amendment') and
                      net_bookings_value__c <> 0 and
                      finance_sub_status__c = 'Booked' and
                      opportunity__c is null
                )
    
        )
 )
 
  
 
 
 -- Start Joins
 select
 x.casenumber,
 a.*
 from
 caselist x,
 left join "SFDC-CASE-W0001-T0001-GROUPED-CASES" a on x.casenumber = a.casenumber
 
 

```

## View Name: `SFDC-CASE-W0001-T0001-GROUPED-CASES`

| Column | Description |
| --- | --- |
| `id_h`| Hybrid Opportunity ID |
| `opportunity__c`| Opportunity ID |
| `casenumber`| Case Number of the Top Case |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local for `id_h` |
