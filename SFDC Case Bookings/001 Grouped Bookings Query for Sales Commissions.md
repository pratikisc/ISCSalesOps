---
title: Opportunity to Case Metrics Mapping.
NOTE: Only Bookings with Commission Processing flag = NULL are included; Only Renewal, Amendment, Transfer included in grouping. Splits are removed from Grouping (Renewal, Amendment, Transfer splits)
View: sfdc-case-w0001-t0001-grouped-cases
Status: Interim View.
---

```sql
-- CREATE VIEW SFDC-CASE-W0001-T0001-GROUPED-CASES AS


WITH id_h__sum AS (
	SELECT
		id_h,
		sum(net_bookings_value__c) AS nbv_local_grouped,
		sum(MRRChangeLocal) AS mrr_change_local_grouped,
		sum(Previous_Monthly_Subscription_Fee__c) AS Previous_Monthly_Subscription_Fee__c_grouped,
		sum(Current_Monthly_Subscription_Fee__c) AS Current_Monthly_Subscription_Fee__c_grouped
		FROM
		"sfdc-case-w0001-t0000-a1-base-table"	
		GROUP BY
		id_h
),
	id_h__case AS (
	SELECT
		id_h,
		opportunity__c,
		casenumber
		from
		"sfdc-case-w0001-t0000-a1-base-table" as basetable
		where rank = 1
	)

		
		

SELECT
a.id_h,
b.opportunity__c,
b.casenumber,
a.nbv_local_grouped,
a.mrr_change_local_grouped,
a.Previous_Monthly_Subscription_Fee__c_grouped,
a.Current_Monthly_Subscription_Fee__c_grouped,
'Grouped Booking Value' AS calculationflag
from
id_h__sum as a
left join id_h__case as b on a.id_h = b.id_h
	    

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


