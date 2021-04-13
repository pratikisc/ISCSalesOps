---
view: '"commissions"."sfdc-case-w0001v2-t0005-gam-kam-vertical-join"'
---

### Note

- Use for GAM and KAM plans (merged)
- Merged GAM and SAFER id so that Kent can use same plan
- Troy will need a separate plan
- _Allocation component of GAM iNet Deals applied here. Attribution in previous step_

```sql

SELECT

a.recordtype,
a.casenumber || ' - GAM' AS casenumber,
a.case_list,
a.nbvlocal_list,
a.dm,
a.gam,
a.kam,
a.safer_rep,
a.gam_sub_territory_id as gam_kam_sub_territory_id,
a.calculationflag,
coalesce( d.allocation, a.allocation) as allocation,
a.exchange_rate_to_usd__c,
a.booked_date__c,
a.casecurrency__c,
a.contract__c,
a.contract_length__c,
coalesce( d.allocation, a.allocation) * a.nbvlocal as nbvlocal,
coalesce( d.allocation, a.allocation) * a.mrrchangelocal as mrrchangelocal,
coalesce( d.allocation, a.allocation) * a.prevmrrlocal as prevmrrlocal,
coalesce( d.allocation, a.allocation) * a.currmrrlocal as currmrrlocal,
a.distributor_commission__c,
a.spiff_commission__c,
a.billing_agent__c,
a.billing_agent__c__text,
a.inet_now_licenses__c,
a.inet_safer_synergy__c__text,
a.type,
a.inet_type__c,
a.finance_sub_status__c,
a.incentive_program_competitive_takeaway__c,
a.incentive_program_qualification__c,
a.shipping_outside_of_owners_territory__c,
a.accountid,
a.firstin_partner_account__c,
a.opportunity__c,
a.inetaccount,
a.oracle_organization__c,
a.commission_processing_flag__c,
a.oracle_order_number__c,
a.oracle_system_number__c,
a.opportunityname,
a.opportunity_number,
a.accountname,
a.partnername,
a.Special_Terms_List__c,
a.autorenewexists,
a.msastatus,
a.msaenddate,
a.msastartdate,
a.msaownername,
a.msaownerid,
a.msaname,
a.msanumber

FROM
"commissions"."sfdc-case-w0001v2-t0005-grouped-and-non-grouped-with-attributes-final" AS a
left join "commissions"."plan-rule-2021-003-t001-gam-international-deals-subscriptions" as d ON a.opportunity_number = d.opportunity_number__c
where gam_sub_territory_id is not null

UNION

SELECT

recordtype,
casenumber || ' - KAM' AS casenumber,
case_list,
nbvlocal_list,
dm,
gam,
kam,
safer_rep,
kam_sub_territory_id as gam_kam_sub_territory_id,
calculationflag,
allocation,
exchange_rate_to_usd__c,
booked_date__c,
casecurrency__c,
contract__c,
contract_length__c,
nbvlocal,
mrrchangelocal,
prevmrrlocal,
currmrrlocal,
distributor_commission__c,
spiff_commission__c,
billing_agent__c,
billing_agent__c__text,
inet_now_licenses__c,
inet_safer_synergy__c__text,
type,
inet_type__c,
finance_sub_status__c,
incentive_program_competitive_takeaway__c,
incentive_program_qualification__c,
shipping_outside_of_owners_territory__c,
accountid,
firstin_partner_account__c,
opportunity__c,
inetaccount,
oracle_organization__c,
commission_processing_flag__c,
oracle_order_number__c,
oracle_system_number__c,
opportunityname,
opportunity_number,
accountname,
partnername,
Special_Terms_List__c,
autorenewexists,
msastatus,
msaenddate,
msastartdate,
msaownername,
msaownerid,
msaname,
msanumber

FROM
"commissions"."sfdc-case-w0001v2-t0005-grouped-and-non-grouped-with-attributes-final"
where kam_sub_territory_id is not null



```
