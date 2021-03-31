---
View: '"commissions"."sfdc-case-w0002-dm-t0003-grouped-and-non-grouped-fact"'
Note: Splits already include in base table at start; Casenumber is no longer unique here.
PK: caseid
---

```
select
null::character varying (200) as id_h,
split_id::character varying (200) as caseid,
teamvalue,
casenumber::character varying (200) as casenumber,
dm_sub_territory_incl_split,
calculationflag::character varying (200) as calculationflag,
allocation::float as allocation,
net_bookings_value__c::float as nbvlocal,
MRRChangeLocal::float as mrrchangelocal,
previous_monthly_subscription_fee__c::float as prevmrrlocal,
current_monthly_subscription_fee__c::float as currmrrlocal,
inet_now_licenses__c::float as inet_now_licenses__c_grouped,
null::character varying (200) as inet_safer_synergy__c_grouped

from
"commissions"."sfdc-case-w0002-dm-t0002-non-grouped-cases"

UNION

select
id_h::character varying (200) as id_h,
caseid::character varying (200) as caseid,
teamvalue,
casenumber::character varying (200) as casenumber,
dm_sub_territory_incl_split,
calculationflag::character varying (200) as calculationflag,
allocation::float as allocation,
nbv_local_grouped::float as nbvlocal,
mrr_change_local_grouped::float as mrrchangelocal,
Previous_Monthly_Subscription_Fee__c_grouped::float as prevmrrlocal,
Current_Monthly_Subscription_Fee__c_grouped::float as currmrrlocal,
inet_now_licenses__c_grouped::float as inet_now_licenses__c_grouped,
inet_safer_synergy__c_grouped::character varying (200) as inet_safer_synergy__c_grouped
from
"commissions"."sfdc-case-w0002-dm-t0001-grouped-cases"

```
