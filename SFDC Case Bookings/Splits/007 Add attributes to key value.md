---
title: Adding attributes (Case Data)
View Name: sfdc-w003-t005-key-value-def1
Status: Interim View
---

```sql



SELECT
    a.id,
    a.casenumber,
    a.teamname,
    a.bookingvalue,
    s.booked_date__c,
    s.finance_sub_status__c,
    s.net_bookings_value__c,
    s.parentid,
    s."type",
    s.inet_type__c,
    s.opportunity__c,
    s.accountid,
    round(a.bookingvalue / s.net_bookings_value__c,2) AS allocation
        
FROM
    
        "sfdc-w003-t004-unpivoted-key-values" a
        LEFT JOIN salesforce_case s ON a.casenumber = s.casenumber
    
ORDER BY
    a.id;
