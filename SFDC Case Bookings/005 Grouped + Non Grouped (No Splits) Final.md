---
view:
---

```sql
select
a.casenumber,
a.calculationflag,
a.allocation,
a.nbvlocal,
a.mrrchangelocal,
a.prevmrrlocal,
a.currmrrlocal,
a.inet_now_licenses__c_grouped,
a.inet_safer_synergy__c_grouped

from
"commissions"."sfdc-case-w0001v2-t0003-grouped-and-non-grouped-fact" AS a
```
