---
title: Base Table
NOTE: Only Bookings with Commission Processing flag = NULL are included; Only Renewal, Amendment, Transfer included in grouping. Splits are removed from Grouping (Renewal, Amendment, Transfer splits)
Status: Interim View.
---

```sql

SELECT
	opportunity__c,
	-- Create hybrid opportunity key for a given case
	opportunity__c || '-' || coalesce(inet_type__c, 'No-iNetType__c') || '-' || to_char( booked_date__c, 'YYYY') || '-' || to_char( booked_date__c, 'MM') AS id_h,
	casenumber,
	coalesce(inet_type__c, 'No-iNetType__c') as inet_type__c, -- Same syntax used in Split Table, to identify Split Cases by id_h at sfdc-w003-t005-splits-key-value-final
	net_bookings_value__c,
	Contract_Length__c,
	Current_Monthly_Subscription_Fee__c,
	Previous_Monthly_Subscription_Fee__c,
	Current_Monthly_Subscription_Fee__c - Previous_Monthly_Subscription_Fee__c as MRRChangeLocal,
	to_char( booked_date__c, 'YYYY') AS y,
	to_char( booked_date__c, 'MM') AS m,
	
	-- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
	row_number() OVER(     
		PARTITION BY id_h
		ORDER BY
		net_bookings_value__c DESC,
		booked_date__c DESC
	) AS rank
	FROM
		salesforce_case
	WHERE
		-- Remember to check this same list in the WITH ( CaseList ) table in Final View
		type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree') and    
		finance_sub_status__c = 'Booked' and
		opportunity__c is not null and
		
		-- !!!! Commission Processing Flag excluded here
		
		Commission_Processing_Flag__c is null

	ORDER BY
		net_bookings_value__c DESC

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
