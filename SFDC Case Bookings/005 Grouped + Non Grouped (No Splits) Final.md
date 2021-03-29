---
view: '""commissions"."sfdc-case-w0001v2-t0005-grouped-and-non-grouped-with-attributes-final"'
---

```sql
select
b.recordtype,
a.casenumber,
b.dm,
b.gam,
b.kam,
b.safer_rep,
b.dm_sub_territory_id,
b.gam_sub_territory_id,
b.kam_sub_territory_id,
b.safer_sub_territory_id,
b.amer_psm_sub_territory_id_named,
a.calculationflag,
a.allocation,
b.exchange_rate_to_usd__c,
b.booked_date__c,
b.casecurrency__c,
b.contract__c,
b.contract_length__c,
coalesce (a.nbvlocal, b.net_bookings_value__c) as nbvlocal,
coalesce (a.mrrchangelocal, b.mrrchangelocal) as mrrchangelocal,
coalesce (a.prevmrrlocal, b.prevmrrlocal) as prevmrrlocal,
coalesce (a.currmrrlocal, b.currmrrlocal) as currmrrlocal,
b.distributor_commission__c,
b.spiff_commission__c,
b.billing_agent__c,
b.billing_agent__c__text,
coalesce (a.inet_now_licenses__c_grouped, b.inet_now_licenses__c) as inet_now_licenses__c,
coalesce (a.inet_safer_synergy__c_grouped, b.inet_safer_synergy__c__text) as inet_safer_synergy__c__text,
b.type,
b.inet_type__c,
b.finance_sub_status__c,
b.incentive_program_competitive_takeaway__c,
b.incentive_program_qualification__c,
b.shipping_outside_of_owners_territory__c,
b.accountid,
b.firstin_partner_account__c,
b.opportunity__c,
b.inetaccount,
b.oracle_organization__c,
b.commission_processing_flag__c,
b.oracle_order_number__c,
b.oracle_system_number__c,
b.opportunityname,
b.opportunity_number,
b.accountname,
b.partnername,
b.Special_Terms_List__c,
b.autorenewexists,
b.msastatus,
b.msaenddate,
b.msastartdate,
b.msaownername,
b.msaname,
b.msanumber


from
"commissions"."sfdc-case-w0001v2-t0003-grouped-and-non-grouped-fact" AS a
left join "commissions"."reference-sfdc-case-attributes-with-plan-attributes" AS b ON a.casenumber = b.casenumber
```
