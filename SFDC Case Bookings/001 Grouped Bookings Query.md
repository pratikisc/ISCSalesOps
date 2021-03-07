---
title: Opportunity to Case Metrics Mapping
description: View for getting the connection between grouped cases metrics to the relevant opportunity. Renewal, Amendment and Transfer% cases are included in the grouping.
Status: Interim View
usage: This is an interim view for downstream calculations on Salesforce booking can be used in many different salesforce reports for a quick connection between `accountid` and the relevant `email`.
---

```sql
-- CREATE VIEW SFDC-CASE-W0001-T0001-GROUPED-CASES AS


-- Base Table for all booked case with an opporunity id
WITH basetable AS (
SELECT
	opportunity__c,

	-- Create hybrid opportunity key for a given case
	opportunity__c || '-' || inet_type__c || '-' || to_char( booked_date__c, 'YYYY') || '-' || to_char( booked_date__c, 'MM') AS id_h,
	casenumber,
	inet_type__c,
	net_bookings_value__c,
	Contract_Length__c,
	Current_Monthly_Subscription_Fee__c,
	Previous_Monthly_Subscription_Fee__c,
	Current_Monthly_Subscription_Fee__c - Previous_Monthly_Subscription_Fee__c as MRRChangeLocal,
	to_char( booked_date__c, 'YYYY') AS y,
	to_char( booked_date__c, 'MM') AS m,
	
	-- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
	rank() OVER(     
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
		opportunity__c is not null

	ORDER BY
		net_bookings_value__c DESC
)

-- Use Base Table to obtain Case KPI Grouped Values
SELECT
basetable.id_h,
basetable.opportunity__c,
basetable.casenumber,
t1.nbv_local_grouped,
t1.mrr_change_local_grouped,
t1.Previous_Monthly_Subscription_Fee__c_grouped,
t1.Current_Monthly_Subscription_Fee__c_grouped,
'Grouped Booking Value' AS calculationflag
from
basetable
left outer join
    (
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
	) t1
	on t1.id_h = basetable.id_h
where
basetable.rank = 1 and
basetable.opportunity__c is not null


	    
	    

```

## View Results Dictionary

## View Name: `SFDC-CASE-W0001-T0001-GROUPED-CASES`

| Column | Description |
| --- | --- |
| `id_h` `key`| Hybrid Opportunity ID |
| `opportunity__c`| Opportunity ID |
| `casenumber` `key`| Case Number of the Top Case |
| `nbv_local_grouped` | Sum of All NBV Local for `id_h` |
| `mrr_change_local_grouped` | Sum of All MRR Change Local grouped by `id_h` |
| `Previous_Monthly_Subscription_Fee__c_grouped`| Sum of Previous MRR Local grouped by `id_h` |
| `Current_Monthly_Subscription_Fee__c_grouped` | Sum of Current MRR Local grouped by `id_h` |
| `calculationflag` | To know if case calculation was grouped by Opportunity |


