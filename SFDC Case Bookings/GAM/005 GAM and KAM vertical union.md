---
view:'"commissions"."sfdc-case-w0001v2-t0005-gam-kam-vertical-join"'
---

### Note

- Use for GAM and KAM plans (merged)
- Merged GAM and SAFER id so that Kent can use same plan
- Troy will need a separate plan

```sql

SELECT

recordtype,
casenumber || ' - GAM' AS casenumber,
case_list,
nbvlocal_list,
dm,
gam,
kam,
safer_rep,
gam_sub_territory_id as gam_kam_sub_territory_id,
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
