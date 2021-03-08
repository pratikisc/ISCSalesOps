---
title: Adding attributes (Case Data)
View Name: sfdc-w003-t005-key-value-def1
Status: Final View
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
    round(a.bookingvalue / s.net_bookings_value__c,2) AS allocation,
    s1.name as opportunityname,
    s1.opportunity_number__c as opportunitynumber,
    s2.name as accountname
        
FROM
    
        "sfdc-w003-t004-unpivoted-key-values" a
        LEFT JOIN salesforce_case s ON a.casenumber = s.casenumber
        LEFT JOIN sfdc_opportunity s1 ON s.opportunity__c = s1.id
        LEFT JOIN salesforce_account s2 ON s.accountid = s2.id
    
ORDER BY
    a.id;
