---
title: Base Table for Cases with Opportunity
NOTE: Only Bookings with Commission Processing flag = NULL are included; Only Renewal, Amendment, Transfer included in grouping. Splits are removed from Grouping (Renewal, Amendment, Transfer splits)
View Name: sfdc-case-w0001-t0000-a1-base-table
Status: Interim View.
---

```sql

SELECT
	a.opportunity__c,
	-- Create hybrid opportunity key for a given case
	a.opportunity__c || '-' || coalesce(a.inet_type__c, 'No-iNetType__c') || '-' || to_char( a.booked_date__c, 'YYYY') || '-' || to_char( a.booked_date__c, 'MM') AS id_h,
	a.casenumber,
	coalesce(a.inet_type__c, 'No-iNetType__c') as inet_type__c, -- Same syntax used in Split Table, to identify Split Cases by id_h at sfdc-w003-t005-splits-key-value-final
	a.net_bookings_value__c,
	a.Contract_Length__c,
	a.Current_Monthly_Subscription_Fee__c,
	a.Previous_Monthly_Subscription_Fee__c,
	a.Current_Monthly_Subscription_Fee__c - a.Previous_Monthly_Subscription_Fee__c as MRRChangeLocal,
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
		left join "sfdc-w003-t005-splits-key-value-final" as j1 ON a.casenumber = j1.casenumber
	WHERE
		-- Remember to check this same list in the WITH ( CaseList ) table in Final View
		a.type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and    
		a.finance_sub_status__c = 'Booked' and
		a.opportunity__c is not null and
		
		-- !!!! Commission Processing Flag excluded here
		
		a.Commission_Processing_Flag__c is null and
		
		-- !!!! Exlclude items that are being split
		j1.casenumber is null


	ORDER BY
		a.net_bookings_value__c DESC

```		
## View Description

| Column | Description |
| --- | --- |
| `id_h` `key`| Hybrid Opportunity ID |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local grouped by `id_h` |
| `Previous_Monthly_Subscription_Fee__c_grouped`| Sum of Previous MRR Local grouped by `id_h` |
| `Current_Monthly_Subscription_Fee__c_grouped` | Sum of Current MRR Local grouped by `id_h` |
| `calculationflag` | To know if case calculation was grouped by Opportunity |