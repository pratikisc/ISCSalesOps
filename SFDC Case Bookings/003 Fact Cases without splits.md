---
View: '"commissions"."sfdc-case-w0001v2-t0003-grouped-and-non-grouped-fact"'
---

```
select
casenumber::character varying (200) as casenumber,
calculationflag::character varying (200) as calculationflag,
allocation::float as allocation,
null::float as nbvlocal,
null::float as mrrchangelocal,
null::float as prevmrrlocal,
null::float as currmrrlocal,
null::float as inet_now_licenses__c_grouped,
null::character varying (200) as inet_safer_synergy__c_grouped

from
"commissions"."sfdc-case-w0001v2-t0002-non-grouped-cases"

UNION

select
casenumber::character varying (200) as casenumber,
calculationflag::character varying (200) as calculationflag,
allocation::float as allocation,

nbv_local_grouped::float as nbvlocal,
mrr_change_local_grouped::float as mrrchangelocal,
Previous_Monthly_Subscription_Fee__c_grouped::float as prevmrrlocal,
Current_Monthly_Subscription_Fee__c_grouped::float as currmrrlocal,
inet_now_licenses__c_grouped::float as inet_now_licenses__c_grouped,
inet_safer_synergy__c_grouped::character varying (200) as inet_safer_synergy__c_grouped
from
"commissions"."sfdc-case-w0001v2-t0001-grouped-cases"

```
