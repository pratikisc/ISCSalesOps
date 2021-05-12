---
title: Base Table for Cases with Opportunity
'!!! IMPORTANT NOTE': Only Bookings with Commission Processing flag = NULL are included; Only Renewal, Term Extension, Amendment, Transfer included in grouping.
View Name: '"commissions"."sfdc-case-w0001v2-t0000-a1-base-table"'
Status: Interim View.
---

Note:
- MRR Change override has been applied here.
```sql

SELECT
	a.opportunity__c,
	-- Create hybrid opportunity key for a given case
	a.opportunity__c || '-' || coalesce(a.inet_type__c, 'No-iNetType__c') || '-' || to_char( a.booked_date__c, 'YYYY') || '-' || to_char( a.booked_date__c, 'MM') AS id_h,
	a.casenumber,
	coalesce(a.inet_type__c, 'No-iNetType__c') as inet_type__c, -- Same syntax used in Split Table, to identify Split Cases by id_h at sfdc-w003-t005-splits-key-value-final
	coalesce(a.net_bookings_value__c,0) as net_bookings_value__c,
	a.Contract_Length__c,
	coalesce(a.Current_Monthly_Subscription_Fee__c,0) as current_monthly_subscription_fee__c,
	coalesce(a.Previous_Monthly_Subscription_Fee__c,0) as previous_monthly_subscription_fee__c,
	
	-- !!! MRR Change Override applied here
	
	coalesce(b.override_mrrchangelocal,
		 coalesce(a.Current_Monthly_Subscription_Fee__c,0) - coalesce(a.Previous_Monthly_Subscription_Fee__c,0)
		 )
		 as MRRChangeLocal,
	case a.inet_safer_synergy__c when true 
            then 1
            else 0
        end as inet_safer_synergy__c__int,
	coalesce(a.inet_now_licenses__c,0) as inet_now_licenses__c,
	to_char( a.booked_date__c, 'YYYY') AS y,
	to_char( a.booked_date__c, 'MM') AS m,
	
	-- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
	row_number() OVER(     
		PARTITION BY a.opportunity__c, coalesce(a.inet_type__c, 'No-iNetType__c'), to_char( a.booked_date__c, 'YYYY'), to_char( a.booked_date__c, 'MM')
		ORDER BY
		a.net_bookings_value__c DESC,
		a.booked_date__c DESC
	) AS rank
	FROM
		salesforce_case as a
		left join "commissions"."reference-sfdc-case-details-override-v2" as b ON a.casenumber = b.casenumber
	
	WHERE
		--- !!! Filter for Grouped Cases: Type, Opportunity__c
		-- Remember to check this same list in the WITH ( CaseList ) table in Final View
		a.type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and    
		a.finance_sub_status__c = 'Booked' and
		a.opportunity__c is not null and
		
		-- !!!! Commission Processing Flag excluded here
		
		a.Commission_Processing_Flag__c is null
		
	ORDER BY
		a.net_bookings_value__c DESC

```
