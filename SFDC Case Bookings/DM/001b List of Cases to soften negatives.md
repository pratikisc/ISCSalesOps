---
View: '"commissions"."sfdc-case-w0002-dm-t0002ref-soften-negatives"'
---

### The case numbers here will have an override on Contract Length to right size the negative impact, or have a flag populated for downstream adjustment on payout

```sql

-- List of id_h that are really individual cases

WITH idh_list AS (
            SELECT
            id_h
            ,count (distinct casenumber) casecount
            FROM   "commissions"."sfdc-case-w0002-dm-t0000-base-table"
            WHERE id_h is not null
            GROUP  BY id_h
            HAVING casecount < 2
       )

SELECT DISTINCT
b.casenumber
FROM idh_list as a
LEFT JOIN "commissions"."sfdc-case-w0002-dm-t0000-base-table" AS b ON a.id_h = b.id_h
WHERE b.net_bookings_value__c < 0



```
