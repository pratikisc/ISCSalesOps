---
View: '"commissions"."sfdc-case-w0002-dm-t0003-grouped-and-non-grouped-fact"'
Note: Splits already include in base table at start; Casenumber is no longer unique here.
---

```
select
split_id::character varying (200) as caseid,
casenumber::character varying (200) as casenumber,
dm_sub_territory_incl_split,
calculationflag::character varying (200) as calculationflag,
allocation::float as allocation,
null::float as nbvlocal,
null::float as mrrchangelocal,
null::float as prevmrrlocal,
null::float as currmrrlocal,
null::float as inet_now_licenses__c_grouped,
null::character varying (200) as inet_safer_synergy__c_grouped

from
"commissions"."sfdc-case-w0002-dm-t0002-non-grouped-cases"

UNION

select
caseid::character varying (200) as caseid,
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
