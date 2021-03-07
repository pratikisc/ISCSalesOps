---
title: Final View for CIQ / Power BI Sales Team
description: Final view to calculate Subscription Bookings
Status: Final View

---

```sql
-- CREATE VIEW "SFDC-CASE-W0001-T0003-FINAL-VIEW" AS


-- UNION to get relevant case numbers
WITH caselist AS (   
        (
            SELECT
                casenumber
            FROM
                "SFDC-CASE-W0001-T0001-GROUPED-CASES" -- Grouped Renewal and Amendment cases by Opportunity
    
        )
        UNION
        (
            SELECT
            casenumber
            FROM
            salesforce_case
            WHERE
                (
                    type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and
                    finance_sub_status__c = 'Booked'
                )
                or
                (
                      type in ('Renewal', 'Amendment') and
                      finance_sub_status__c = 'Booked' and
                      opportunity__c is null
                )
    
        )
 )
 

 -- Start Joins
select
 x.casenumber,
 a.nbv_local_grouped,
 a.Previous_Monthly_Subscription_Fee__c_grouped	,
 a.Current_Monthly_Subscription_Fee__c_grouped,
 a.mrr_change_local_grouped,
 COALESCE(a.mrr_change_local_grouped, b.mrrchangelocal,0) as mrrchangelocaloverride,
 COALESCE(a.nbv_local_grouped,b.net_bookings_value__c,0) as nbvlocaloverride,
 COALESCE(a.Previous_Monthly_Subscription_Fee__c_grouped,b.Previous_Monthly_Subscription_Fee__c,0) as Previous_Monthly_Subscription_Fee__c_override,
 COALESCE(a.Current_Monthly_Subscription_Fee__c_grouped,b.Current_Monthly_Subscription_Fee__c,0) as Current_Monthly_Subscription_Fee__c_override,
 a.calculationflag,
 b.*
 
from
 caselist as x
 left outer join "SFDC-CASE-W0001-T0001-GROUPED-CASES" as a ON x.casenumber = a.casenumber
 inner join "sfdc-case-w0001-t0002-case-attributes" as b ON x.casenumber = b.casenumber
 
 
 -- Use later.. COALESCE(b.net_bookings_value__c_override, net_bookings_value__c) AS 

```

## View Name: `"SFDC-CASE-W0001-T0003-FINAL-VIEW"`


| Column | Description |
| --- | --- |
| `id_h`| Hybrid Opportunity ID |
| `opportunity__c`| Opportunity ID |
| `casenumber`| Case Number of the Top Case |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local for `id_h` |
