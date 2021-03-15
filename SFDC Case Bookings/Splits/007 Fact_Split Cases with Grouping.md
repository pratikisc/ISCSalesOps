---
View: sfdc-w003v1-t005-fact-split-cases-grouped
NOTE: Check the where clause on edits
---


```

-- Group split case table by id_h || team

-- Get id_h against the case

with getid_h as (
                select
                b.opportunity__c || '-' || coalesce(b.inet_type__c, 'No-iNetType__c')  || '-' || to_char( b.booked_date__c, 'YYYY') || '-' || to_char( b.booked_date__c, 'MM') ||  a.teamvalue AS id_hs,
                a.id,
                a.casenumber,
                a.teamvalue,
                a.bookingvalue,
                b.net_bookings_value__c,
		b.inet_now_licenses__c,
		case b.inet_safer_synergy__c when true 
			then 1
			else 0
			end as inet_safer_synergy__c__int
                               
                from
                
                "sfdc-w003v1-t004-unpivoted-key-values" a
                
                LEFT JOIN salesforce_case b ON a.casenumber = b.casenumber
                
                WHERE
                      -- !!!! Filter for only cases that will be grouped here
                      -- Remember to check this same list in the WITH ( CaseList ) table in Final View
                      b.type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and    
                      b.finance_sub_status__c = 'Booked' and
                      b.opportunity__c is not null and
                      a.teamvalue not like 'GAM-%'
                order by b.net_bookings_value__c
                      
                ),
                
            getrowid as (
              select
                b.opportunity__c || '-' || coalesce(b.inet_type__c, 'No-iNetType__c')  || '-' || to_char( b.booked_date__c, 'YYYY') || '-' || to_char( b.booked_date__c, 'MM')||  a.teamvalue AS id_hs,
                a.teamvalue,
                a.id,
		a.casenumber,
                row_number() OVER(     
                  PARTITION BY b.opportunity__c, coalesce(b.inet_type__c, 'No-iNetType__c'), to_char( b.booked_date__c, 'YYYY'), to_char( b.booked_date__c, 'MM'), a.teamvalue
                  ORDER BY
                  b.net_bookings_value__c DESC,
                  b.booked_date__c DESC
	                ) AS rank
              
              from
              "sfdc-w003v1-t004-unpivoted-key-values" a
              LEFT JOIN salesforce_case b ON a.casenumber = b.casenumber
              
              WHERE
                      -- !!!! Filter for only cases that will be grouped here (SAME AS ABOVE)
                      -- Remember to check this same list in the WITH ( CaseList ) table in Final View
                      b.type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and    
                      b.finance_sub_status__c = 'Booked' and
                      b.opportunity__c is not null and
                      a.teamvalue not like 'GAM-%'
          )
    
    
    
select
a.id_hs,
b.id,
b.casenumber,
b.teamvalue,
a.bookingvalue_g,
a.nbvgrouped,
a.bookingvalue_g/a.nbvgrouped as allocation,
a.inet_now_licenses__c_grouped,
case when a.inet_safer_synergy__c_grouped  > 0 
            then 'true'
            else 'false'
        end as inet_safer_synergy__c_grouped,
a.calculationflag

from
	(    select
		id_hs,
		sum(bookingvalue) as bookingvalue_g,
		sum(net_bookings_value__c)  as nbvgrouped,
		sum(inet_now_licenses__c) as inet_now_licenses__c_grouped,
		sum(inet_safer_synergy__c__int) as inet_safer_synergy__c_grouped,
		'Split / Grouped' as calculationflag

		from getid_h
		group by id_hs


    		having nbvgrouped <> 0

    		order by id_hs, nbvgrouped
	) as a
left join getrowid b ON a.id_hs = b.id_hs
where b.rank = 1


                
    
                
