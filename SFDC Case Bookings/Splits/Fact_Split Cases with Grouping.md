---
View: sfdc-w003-t005-fact-split-cases-grouped
---


```

-- Group split case table by id_h || team

-- Get id_h against the case

with getid_h as (
                select
                b.opportunity__c || '-' || b.inet_type__c || '-' || to_char( b.booked_date__c, 'YYYY') || '-' || to_char( b.booked_date__c, 'MM') AS id_h
                a.id,
                a.casenumber,
                a.teamname,
                a.bookingvalue,
                b.net_bookings_value__c
                               
                from
                
                "sfdc-w003-t004-unpivoted-key-values" a
                
                LEFT JOIN salesforce_case b ON a.casenumber = b.casenumber
                )
select
  id_h,
  teamname,
  sum(bookingvalue) as bookingvalue_g,
  sum(net_bookings_value__c) / sum(bookingvalue) as allocation

from getid_h

group by id_h, teamname
                
    
                
