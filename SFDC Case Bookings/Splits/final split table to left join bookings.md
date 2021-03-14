---
title: Adding attributes to Split Key Values
View Name: "sfdc-w003-t005-final-metrics-to-join"
Note: GAM Team Names have been excluded from split
Status: Final View
---

```sql
WITH dim_case AS (
    select
        casenumber,
        booked_date__c,
        type,
        finance_sub_status__c,
        net_bookings_value__c,
        coalesce(inet_type__c, 'No-iNetType__c') as inet_type__c,
        opportunity__c    
    from
        salesforce_case
    where
        finance_sub_status__c = 'Booked'
   ),
   dim_opportunity AS (
    select
    id,
    opportunity_number__c,
    name
    from
    sfdc_opportunity
   )


SELECT
    s.opportunity__c || '-' || s.inet_type__c || '-' || to_char( s.booked_date__c, 'YYYY') || '-' || to_char( s.booked_date__c, 'MM') AS id_h,
    a.id,
    a.casenumber,
    a.teamname,
    a.bookingvalue,
    s.net_bookings_value__c,
    ('Split Case')::character varying (200) as calculationflag,
    round(a.bookingvalue / s.net_bookings_value__c,2) AS allocation,
    type,
    finance_sub_status__c
        
FROM
    
        "sfdc-w003-t004-unpivoted-key-values" a
        LEFT JOIN dim_case s ON a.casenumber = s.casenumber
        LEFT JOIN dim_opportunity s1 ON s.opportunity__c = s1.id
WHERE
    a.teamname not like 'GAM%'
ORDER BY
    a.id;
