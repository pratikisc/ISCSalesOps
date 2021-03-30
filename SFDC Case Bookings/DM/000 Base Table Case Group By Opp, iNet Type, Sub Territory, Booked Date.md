---
This View: '"commissions"."sfdc-case-w0002-dm-t0000-base-table"'
'!!! Known case of non unique Foreign Key': Join with split table that will create extra rows for each case
Goal: Group Case By Opp, Create Split Rows with Allocation amount
---

```sql
SELECT
	a.opportunity__c,
	-- Create hybrid opportunity key for a given case
	a.opportunity__c || '-' || coalesce(b.dm_sub_territory_incl_split,0) || '-' || coalesce(a.inet_type__c, 'No-iNetType__c') || '-' || to_char( a.booked_date__c, 'YYYY') || '-' || to_char( a.booked_date__c, 'MM') AS id_h,
	a.casenumber,
	b.split_id,
	b.allocation,
	b.dm_sub_territory_incl_split,
	coalesce(a.inet_type__c, 'No-iNetType__c') as inet_type__c, -- Same syntax used in Split Table, to identify Split Cases by id_h at sfdc-w003-t005-splits-key-value-final
	coalesce(b.allocation* a.net_bookings_value__c,0) as net_bookings_value__c,
	a.Contract_Length__c,
	coalesce(b.allocation* a.Current_Monthly_Subscription_Fee__c,0) as current_monthly_subscription_fee__c,
	coalesce(b.allocation* a.Previous_Monthly_Subscription_Fee__c,0) as previous_monthly_subscription_fee__c,
	coalesce(b.allocation* a.Current_Monthly_Subscription_Fee__c,0) - coalesce(b.allocation* a.Previous_Monthly_Subscription_Fee__c,0) as MRRChangeLocal,
	case a.inet_safer_synergy__c when true 
            then 1
            else 0
        end as inet_safer_synergy__c__int,
	coalesce(b.allocation* a.inet_now_licenses__c,0) as inet_now_licenses__c,
	to_char( a.booked_date__c, 'YYYY') AS y,
	to_char( a.booked_date__c, 'MM') AS m,
	
	-- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
	row_number() OVER(     
		PARTITION BY a.opportunity__c, coalesce(b.dm_sub_territory_incl_split,0), coalesce(a.inet_type__c, 'No-iNetType__c'), to_char( a.booked_date__c, 'YYYY'), to_char( a.booked_date__c, 'MM')
		ORDER BY
		a.net_bookings_value__c DESC,
		a.booked_date__c DESC
	) AS rank
	FROM
		salesforce_case as a
		
		-- !!! Known case of non unique foreign key
		left join "commissions"."reference-sfdc-case-dm-subterritory-incl-splits" as b ON a.casenumber = b.casenumber
	
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
