---
View Name: '"commissions"."sfdc-case-w0001v2-t0003-grouped-and-non-grouped-split-fact"'
---
```
select
b.id,
a.casenumber,
b.teamvalue,
a.inet_now_licenses__c_grouped,
a.inet_safer_synergy__c_grouped,

coalesce(b.calculationflag,'No Split') || ' - ' || a.calculationflag as calculationflag,
coalesce(b.allocation::float,1) as allocation,
coalesce(b.allocation::float,1) * a.nbvlocal as nbvlocal,
coalesce(b.allocation::float,1) * a.mrrchangelocal as mrrchangelocal,
coalesce(b.allocation::float,1) * a.prevmrrlocal as prevmrrlocal,
coalesce(b.allocation::float,1) * a.currmrrlocal as currmrrlocal


from
"commissions"."sfdc-case-w0001v2-t0003-grouped-and-non-grouped-fact" a
left join "commissions"."sfdc-w003v2-t005-split-ref-table" b
on a.casenumber = b.casenumber

```
