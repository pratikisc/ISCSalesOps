---
View: sfdc-w003-t005-fact-split-cases-grouped
NOTE: Check the where clause on edits
---


```

-- Group split case table by id_h || team

-- Get id_h against the case

with getid_h as (
                select
                b.opportunity__c || '-' || coalesce(b.inet_type__c, 'No-iNetType__c')  || '-' || to_char( b.booked_date__c, 'YYYY') || '-' || to_char( b.booked_date__c, 'MM') AS id_h,
                a.id,
                a.casenumber,
                a.teamname,
                a.bookingvalue,
                b.net_bookings_value__c
                               
                from
                
                "sfdc-w003-t004-unpivoted-key-values" a
                
                LEFT JOIN salesforce_case b ON a.casenumber = b.casenumber
                
                WHERE
                      -- !!!! Filter for only cases that will be grouped here
                      -- Remember to check this same list in the WITH ( CaseList ) table in Final View
                      b.type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and    
                      b.finance_sub_status__c = 'Booked' and
                      b.opportunity__c is not null and
                      a.teamname not like 'GAM-%'
                      
                )
select
  id_h,
  teamname,
  sum(bookingvalue) as bookingvalue_g,
  sum(net_bookings_value__c)  as nbvgrouped,
  'Split / Grouped' as calculationflag

from getid_h

group by id_h, teamname


having nbvgrouped <> 0

order by id_h, nbvgrouped 


                
    
                
