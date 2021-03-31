---
View: '"commissions"."sfdc-case-w0002-dm-t0002ref-soften-negatives"'
---

### The case numbers here will have an override on Contract Length to right size the negative impact, or have a flag populated for downstream adjustment on payout

```sql

-- List of grouped cases that are really individual cases

WITH idh_list AS (
                        
            SELECT DISTINCT
            b.casenumber
            FROM
            (
                        SELECT
                        id_h
                        ,count (distinct casenumber) casecount
                        FROM   "commissions"."sfdc-case-w0002-dm-t0000-base-table"
                        WHERE id_h is not null
                        GROUP  BY id_h
                        HAVING casecount < 2
            ) as a
            LEFT JOIN "commissions"."sfdc-case-w0002-dm-t0001-grouped-cases" AS b ON a.id_h = b.id_h
            WHERE b.nbv_local_grouped < 0
)

, solocases AS (
            
            SELECT casenumber
            from
            salesforce_case
            where
            net_bookings_value__c < 0
            and
            (
                        (
                                    type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
                                    finance_sub_status__c = 'Booked' and
                                    Commission_Processing_Flag__c is null
                        )
                        or
                        (
                                    type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
                                    finance_sub_status__c = 'Booked' and
                                    opportunity__c is null and
                                    Commission_Processing_Flag__c is null
                        )
             )
             
 SELECT DISTINCT casenumber FROM idh_list
 UNION
 SELECT DISTINCT casenumber from solocases

```
