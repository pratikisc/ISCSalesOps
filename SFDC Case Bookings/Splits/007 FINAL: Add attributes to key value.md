---
title: Adding attributes to Split Key Values
View Name: sfdc-w003-t005-splits-key-value-final
Note: This view is referenced by the Primary Case Booking Query
Status: Final View
---

```sql
WITH dim_case AS (
    select
    casenumber,
    booked_date__c,
    finance_sub_status__c,
    net_bookings_value__c,
    parentid,
    type,
    coalesce(inet_type__c, 'No-iNetType__c') as inet_type__c,
    coalesce(opportunity__c, 'No-Opp') as opportunity__c,
    accountid
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
    s.booked_date__c,
    s.finance_sub_status__c,
    s.net_bookings_value__c,
    s.parentid,
    s.type,
    s.inet_type__c,
    s.opportunity__c,
    s.accountid,
    round(a.bookingvalue / s.net_bookings_value__c,2) AS allocation,
    s1.name as opportunityname,
    s1.opportunity_number__c as opportunitynumber,
    s2.name as accountname
        
FROM
    
        "sfdc-w003-t004-unpivoted-key-values" a
        LEFT JOIN dim_case s ON a.casenumber = s.casenumber
        LEFT JOIN dim_opportunity s1 ON s.opportunity__c = s1.id
        LEFT JOIN salesforce_account s2 ON s.accountid = s2.id
    
ORDER BY
    a.id;
