---
title: Account to Email Mapping
description: View for getting the connection between Salesforce `accountid` to the relevant user `email`.
requirements: Collect the `Account`, `Contact` and `Lead` objects with the Panoply Salesforce data source.
usage: This view can be used in many different salesforce reports for a quick connection between `accountid` and the relevant `email`.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. Different filters can be added throughout the query, either in the subquery or by adding a `WHERE` clause in the final query.
---

```sql
-- CREATE VIEW salesforce_account_to_email AS

-- The first WITH returns a table for All Cases grouped by a hybrid Opportunity Key (Opp ID, iNet Type, Booked Date Year, Booked Date Month)

WITH OppId_CaseKPIGrouped AS (
	SELECT
	dt1.opportunity__c || '-' || dt1.inet_type__c || '-' || dt1.y || '-' || dt1.m AS id,
	sum(dt1.net_bookings_value__c) AS nbv_local_grouped
	FROM
	    (
		SELECT
		    salesforce_case.opportunity__c,
		    salesforce_case.casenumber,
		    salesforce_case.inet_type__c,
		    salesforce_case.net_bookings_value__c,
		    to_char( booked_date__c, 'YYYY') AS y,
		    to_char( booked_date__c, 'MM') AS m,
		    -- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
		    rank() OVER(     
			PARTITION BY opportunity__c,
			to_char( booked_date__c, 'YYYY'),
			to_char( booked_date__c, 'MM'),
			inet_type__c
			ORDER BY
			net_bookings_value__c DESC
		    ) AS rank
		FROM
		    salesforce_case
		WHERE
		    type in ('Renewal', 'Amendment') and
		    net_bookings_value__c <> (0) and
		    finance_sub_status__c = 'Booked'

		ORDER BY
		    rank DESC,
		    opportunity__c,
		    net_bookings_value__c DESC
	    ) dt1
	WHERE
	    opportunity__c IS NOT NULL
	GROUP BY
	    dt1.opportunity__c,
	    dt1.y,
	    dt1.m,
	    dt1.inet_type__c
)


```

## View Results Dictionary

| Column | Description |
| --- | --- |
| `email`| Opportunity Email |
| `accountid`| Salesforce Account ID |
| `account_name`| Account Name |

## Example: New Opportunities Summary

[See Query Details Here](https://github.com/panoplyio/sql-library/blob/master/salesforce/queries/new_opps_summary.md)
