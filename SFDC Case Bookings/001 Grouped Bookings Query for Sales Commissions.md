---
title: Opportunity to Case Metrics Mapping.
'!!! IMPORTANT NOTE': Only Bookings with Commission Processing flag = NULL are included; Only Renewal, Amendment, Transfer included in grouping.
View: '"commissions"."sfdc-case-w0001v2-t0001-grouped-cases"'
Status: Interim View.
---

```sql

WITH id_h__sum AS (
	SELECT
		id_h,
		sum(net_bookings_value__c) AS nbv_local_grouped,
		sum(MRRChangeLocal) AS mrr_change_local_grouped,
		sum(Previous_Monthly_Subscription_Fee__c) AS Previous_Monthly_Subscription_Fee__c_grouped,
		sum(Current_Monthly_Subscription_Fee__c) AS Current_Monthly_Subscription_Fee__c_grouped,
		sum(inet_now_licenses__c) AS inet_now_licenses__c_grouped,
		sum(inet_safer_synergy__c__int) AS inet_safer_synergy__c_grouped
		FROM
		"commissions"."sfdc-case-w0001v2-t0000-a1-base-table"	
		GROUP BY
		id_h
),
	id_h__case AS (
	SELECT
		id_h,
		opportunity__c,
		casenumber
		from
		"commissions"."sfdc-case-w0001v2-t0000-a1-base-table" as basetable
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
a.inet_now_licenses__c_grouped,

case when a.inet_safer_synergy__c_grouped  > 0 
            then 'true'
            else 'false'
        end as inet_safer_synergy__c_grouped,

'Grouped Booking Value' AS calculationflag
from
id_h__sum as a
left join id_h__case as b on a.id_h = b.id_h
	    

```


