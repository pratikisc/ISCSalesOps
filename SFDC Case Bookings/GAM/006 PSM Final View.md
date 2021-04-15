---
view: '"commissions"."sfdc-case-w0001v2-t0006-psm"'
---
### Note

- _Includes UNION for Total Safety and PSM Geo iNet Launch Bookings_

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
a.amer_psm_sub_territory_id_named,
a.calculationflag,
a.allocation,
a.exchange_rate_to_usd__c,
a.booked_date__c,
a.casecurrency__c,
a.contract__c,
a.contract_length__c,
a.nbvlocal,
a.mrrchangelocal,
a.prevmrrlocal,
a.currmrrlocal,
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
where amer_psm_sub_territory_id_named is not null

```
