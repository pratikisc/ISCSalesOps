---
View Name:
---
```
select
a.id_h,
b.id,
a.opportunity__c,
a.casenumber,

a.inet_now_licenses__c_grouped,
a.inet_safer_synergy__c_grouped,
a.mrr_change_local_grouped,
coalesce(b.calculationflag::character varying (200), a.calculationflag::character varying (200)) as calculationflag,
coalesce(b.allocation::float,1) as allocation,
coalesce(b.allocation::float,1) * a.nbv_local_grouped as nbv__,
coalesce(b.allocation::float,1) * a.mrr_change_local_grouped as mrr_change__,
coalesce(b.allocation::float,1) * a.previous_monthly_subscription_fee__c_grouped as prev_monthly__,
coalesce(b.allocation::float,1) * a.current_monthly_subscription_fee__c_grouped as curr_monthly__


from
"commissions"."sfdc-case-w0001v2-t0001-grouped-cases" a
left join "commissions"."sfdc-w003v2-t005-split-ref-table" b
on a.casenumber = b.casenumber

```
