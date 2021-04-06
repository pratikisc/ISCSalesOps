---
View: '"commissions"."reference-sfdc-case-attributes-with-plan-attributes"'
Note: !!! Plan overrides / sub territory allocation applied here
PK: casenumber
---

```sql
 
WITH
    dim_opportunity AS (
        select distinct
        id,
        name,
        opportunity_number__c
        from
        sfdc_opportunity
    ),
    dim_account AS (
        select distinct
        id,
        name
        from
        salesforce_account
    ),
   dim_contract AS (
       
       
       select
       a1.id,
       a1.Special_Terms_List__c,
       case when a1.special_terms_list__c like '%Auto-Renewal%' then true else false end as autorenewexists,
       a1.parent_contract__c,
       a2.status msastatus,
       a2.contract_end_date__c msaenddate,
       a2.startdate msastartdate,
       a2.OwnerId as msaownerid,
       a2.ContractNumber as msanumber,
       a2.name as msaname,
       a3.full_name__c as msaownername
       from salesforce_contract as a1
           left outer join salesforce_contract as a2 on a1.parent_contract__c = a2.id
           left outer join salesforce_user as a3 on a2.ownerid = a3.id
    ),
  dim_user AS (
       select
       id,
       full_name__c
       from
       salesforce_user
    ),
  
  dim_recordtype AS (
       SELECT
       id,
       name
       FROM salesforce_recordtype
  ),
  
  -- !!! Override Table from Airtable
  psm_plan_ov AS (
     select
        trim(from __opportunity_number) as opp_number,
        __partner_sub_territory_id
     from
        "override"."sheets_join sfdc case overrides_join_sfdc inet case - opportunity overlays"
     where
        "qc checked for unique pk?" is true
 )
 
SELECT
      n.name as recordtype,
      a.casenumber,
      
      g.full_name__c as dm,
      f.full_name__c as gam,
      a.key_account_manager__c as kam,
      a.safer_rep__c as safer_rep,
      
      i.__sub_territory_id as dm_sub_territory_id,
      j.__sub_territory_id as gam_sub_territory_id,
      k.__sub_territory_id as kam_sub_territory_id,
      
      
      -- !!! Plan override to allocate credit to Troy for APAC SAFER Launch Deals
      case
       when l.__sub_territory_id = 7013 and a.type IN ('Launch', 'Amendment') then 6667013::bigint
       else l.__sub_territory_id
       end as safer_sub_territory_id,
      
      
      coalesce(m.__partner_sub_territory_id::integer, h.amer_psm_sub_territory::integer) as amer_psm_sub_territory_id_named,
      
      
      net_bookings_value__c,
      exchange_rate_to_usd__c,
      coalesce(o.booked_date__c_override, a.booked_date__c) as booked_date__c,
      casecurrency__c,
      contract__c,
      contract_length__c,
      coalesce(current_monthly_subscription_fee__c,0) as currentmrrlocal,
      coalesce(previous_monthly_subscription_fee__c,0) as prevmrrlocal,
      coalesce(current_monthly_subscription_fee__c,0) - coalesce(previous_monthly_subscription_fee__c,0) as mrrchangelocal,
      coalesce(distributor_commission__c,0) as distributor_commission__c,
      coalesce(spiff_commission__c,0) as spiff_commission__c,
      type,
      inet_type__c,
      coalesce(inet_now_licenses__c,0) as inet_now_licenses__c,
      finance_sub_status__c,
      incentive_program_competitive_takeaway__c,
      incentive_program_qualification__c,
      shipping_outside_of_owners_territory__c,
      accountid,
      firstin_partner_account__c,
      billing_agent__c,
      case billing_agent__c when true 
            then 'true'::character varying (5)
            else 'false'::character varying (5)
        end as billing_agent__c__text,
      inet_safer_synergy__c,
      case inet_safer_synergy__c when true 
            then 'true'::character varying (5)
            else 'false'::character varying (5)
        end as inet_safer_synergy__c__text,
      
      
      a.opportunity__c,
      a.siebel_contract__c	as inetaccount,
      oracle_organization__c,
      commission_processing_flag__c,
      oracle_order_number__c,
      oracle_system_number__c,
      b.name as opportunityname,
      b.opportunity_number__c as opportunity_number,
      c.name as accountname,
      d.name as partnername,
      e.Special_Terms_List__c,
      coalesce(e.autorenewexists, false) as autorenewexists,
      e.msastatus,
      e.msaenddate,
      e.msastartdate,
      e.msaownername,
      e.msaownerid,
      e.msaname,
      e.msanumber
      
from
      salesforce_case as a
      left join dim_opportunity as b ON a.opportunity__c = b.id
      left join dim_account as c ON a.accountid = c.id
      left join dim_account as d ON a.firstin_partner_account__c = d.id
      left join dim_contract as e ON a.contract__c = e.id
      left join dim_user as f ON a.nam__c = f.id
      left join dim_user as g ON a.dm__c = g.id
      left join "commissions"."plan-rule-2021-001-t001-amer-psm-inet-launches" as h ON a.casenumber = h.casenumber
      left join "territory"."sheets_join_territory_join sfdc case dm name" as i ON a.dm__c = i.__dm__c
      left join "territory"."sheets_join_territory_join sfdc case gam" as j ON a.nam__c = j.__nam__c
      left join "territory"."sheets_join_territory_join sfdc case kam" as k ON a.key_account_manager__c = k.__key_account_manager__c
      left join "territory"."sheets_join_territory_join sfdc case safer_rep__c" as l ON a.safer_rep__c = l.__safer_rep__c
      -- !!! Americas PSM Plan override
      left join psm_plan_ov as m ON b.opportunity_number__c = m.opp_number
      left join dim_recordtype as n ON a.recordtypeid = n.id
      -- !!! Greater China FP Date override for fiscal / payout date adjustment
      left join "commissions"."reference-sfdc-case-date-override-greater-china" as o ON a.casenumber = o.casenumber

      where
      finance_sub_status__c = 'Booked' 


```

