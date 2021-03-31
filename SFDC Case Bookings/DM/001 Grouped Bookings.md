---
title: Opportunity to Case Metrics Mapping.
'!!! IMPORTANT NOTE': Only Bookings with Commission Processing flag = NULL are included
View: '"commissions"."sfdc-case-w0002-dm-t0001-grouped-cases"'
PK: caseid
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
		"commissions"."sfdc-case-w0002-dm-t0000-base-table"	
		GROUP BY
		id_h
),
	id_h__case AS (
	SELECT
		id_h,
		opportunity__c,
		split_id,
		allocation,
		teamvalue,
		dm_sub_territory_incl_split,
		casenumber
		from
		"commissions"."sfdc-case-w0002-dm-t0000-base-table"
		where rank = 1
	)

		
		

SELECT
a.id_h,
coalesce(b.split_id::character varying (200), b.casenumber::character varying (200) ) as caseid,
b.opportunity__c,
b.casenumber,
b.teamvalue,
b.dm_sub_territory_incl_split,
a.nbv_local_grouped,
a.mrr_change_local_grouped,
a.Previous_Monthly_Subscription_Fee__c_grouped,
a.Current_Monthly_Subscription_Fee__c_grouped,
a.inet_now_licenses__c_grouped,

case when a.inet_safer_synergy__c_grouped  > 0 
            then 'true'
            else 'false'
        end as inet_safer_synergy__c_grouped,

'Grouped Booking Value' AS calculationflag,
coalesce(b.allocation,1) as allocation
from
id_h__sum as a
left join id_h__case as b on a.id_h = b.id_h
	    

```


