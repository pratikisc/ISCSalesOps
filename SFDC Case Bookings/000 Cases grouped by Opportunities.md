---
title: Cases grouped by Opportunity Id_h
NOTE: Filters applied for Commission Processing flag = NULL; Only Renewal, Amendment, Transfer included in grouping. Splits are removed at a later stage.
View: sfdc-case-w0001-t0000-casemetricsgroupedbyopp
Status: Interim View.
---

```sql

-- Base Table for all booked case with an opporunity id
WITH basetable AS (
SELECT
	opportunity__c,

	-- Create hybrid opportunity key for a given case
	opportunity__c || '-' || inet_type__c || '-' || to_char( booked_date__c, 'YYYY') || '-' || to_char( booked_date__c, 'MM') AS id_h,
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
)

SELECT
		basetable.id_h,
		sum(basetable.net_bookings_value__c) AS nbv_local_grouped,
		sum(basetable.MRRChangeLocal) AS mrr_change_local_grouped,
		sum(basetable.Previous_Monthly_Subscription_Fee__c) AS Previous_Monthly_Subscription_Fee__c_grouped,
		sum(basetable.Current_Monthly_Subscription_Fee__c) AS Current_Monthly_Subscription_Fee__c_grouped
		FROM
		basetable	
		GROUP BY
		id_h
```		
## View Name: `sfdc-case-w0001-t0000-casemetricsgroupedbyopp`

| Column | Description |
| --- | --- |
| `id_h` `key`| Hybrid Opportunity ID |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local grouped by `id_h` |
| `Previous_Monthly_Subscription_Fee__c_grouped`| Sum of Previous MRR Local grouped by `id_h` |
| `Current_Monthly_Subscription_Fee__c_grouped` | Sum of Current MRR Local grouped by `id_h` |
| `calculationflag` | To know if case calculation was grouped by Opportunity |
