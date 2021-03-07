---
title: Account to Email Mapping
description: View for getting the connection between Salesforce `accountid` to the relevant user `email`.
requirements: Collect the `Account`, `Contact` and `Lead` objects with the Panoply Salesforce data source.
usage: This view can be used in many different salesforce reports for a quick connection between `accountid` and the relevant `email`.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. Different filters can be added throughout the query, either in the subquery or by adding a `WHERE` clause in the final query.
---

```sql
-- CREATE VIEW salesforce_account_to_email AS

WITH basetable AS (
SELECT
	opportunity__c,

	-- Create hybrid opportunity key for a given case
	opportunity__c || '-' || inet_type__c || '-' || to_char( booked_date__c, 'YYYY') || '-' || to_char( booked_date__c, 'MM') AS id_h,
	casenumber,
	inet_type__c,
	net_bookings_value__c,
	to_char( booked_date__c, 'YYYY') AS y,
	to_char( booked_date__c, 'MM') AS m,
	
	-- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
	rank() OVER(     
		PARTITION BY id_h
		ORDER BY
		net_bookings_value__c DESC
	) AS rank
	FROM
		salesforce_case
	WHERE
		type in ('Renewal', 'Amendment') and
		net_bookings_value__c <> 0 and
		finance_sub_status__c = 'Booked' and
		opportunity__c is not null

	ORDER BY
		net_bookings_value__c DESC
)

-- Use Base Table to obtain Case KPI Grouped Values

WITH OppId_CaseKPIGrouped AS (
	SELECT
	id_h,
	sum(net_bookings_value__c) AS nbv_local_grouped
	FROM
	basetable
	GROUP BY
	id_h
)












-- Case KPIs grouped by a hybrid Opportunity Key (Opp ID, iNet Type, Booked Date Year, Booked Date Month)
WITH OppId_CaseKPIGrouped AS (
	SELECT
	dt1.id_h as id,
	sum(dt1.net_bookings_value__c) AS nbv_local_grouped
	FROM
	    (
		SELECT
		    opportunity__c,
		    -- Create hybrid opportunity key for a given case
		    opportunity__c || '-' || inet_type__c || '-' || to_char( booked_date__c, 'YYYY') || '-' || to_char( booked_date__c, 'MM') AS id_h,
		    casenumber,
		    inet_type__c,
		    net_bookings_value__c,
		    to_char( booked_date__c, 'YYYY') AS y,
		    to_char( booked_date__c, 'MM') AS m,
		    -- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
		    rank() OVER(     
			PARTITION BY id_h
			ORDER BY
			net_bookings_value__c DESC
		    ) AS rank
		FROM
		    salesforce_case
		WHERE
		    type in ('Renewal', 'Amendment') and
		    net_bookings_value__c <> 0 and
		    finance_sub_status__c = 'Booked'

		ORDER BY
		    rank DESC,
		    opportunity__c,
		    net_bookings_value__c DESC
	    ) dt1
	WHERE
	    opportunity__c IS NOT NULL
	GROUP BY
	    dt1.id_h
)

-- Same query as above, except to obtain Case Number for Opportunity ID (Hybrid) with rop rank (Distinct)

SELECT DISTINCT
	dt1.id_h as id,
	dt1.casenumber
	FROM
	    (
		SELECT
		    opportunity__c,
		    -- Create hybrid opportunity key for a given case
		    opportunity__c || '-' || inet_type__c || '-' || to_char( booked_date__c, 'YYYY') || '-' || to_char( booked_date__c, 'MM') AS id_h,
		    casenumber,
		    inet_type__c,
		    net_bookings_value__c,
		    to_char( booked_date__c, 'YYYY') AS y,
		    to_char( booked_date__c, 'MM') AS m,
		    -- for a given Opportunity Key, determine the case with the top rank as determined by highest NBV
		    rank() OVER(     
			PARTITION BY id_h
			ORDER BY
			net_bookings_value__c DESC
		    ) AS rank
		FROM
		    salesforce_case
		WHERE
		    type in ('Renewal', 'Amendment') and
		    net_bookings_value__c <> 0 and
		    finance_sub_status__c = 'Booked'

		ORDER BY
		    rank DESC,
		    opportunity__c,
		    net_bookings_value__c DESC
	    ) dt1
	WHERE
	    dt1.opportunity__c IS NOT NULL and
	    dt1.rank = 1
	    
	    

```

## View Results Dictionary

| Column | Description |
| --- | --- |
| `email`| Opportunity Email |
| `accountid`| Salesforce Account ID |
| `account_name`| Account Name |

## Example: New Opportunities Summary

[See Query Details Here](https://github.com/panoplyio/sql-library/blob/master/salesforce/queries/new_opps_summary.md)
