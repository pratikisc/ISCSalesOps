---
View: '"commissions"."reference-sfdc-case-date-override-greater-china"'
---

_This will join with_

`"commissions"."reference-sfdc-case-attributes-with-plan-attributes"` in [0000 Primary join CaseAttributes incl Sub Territories.md](https://github.com/pratikshr/ISCSalesOps/blob/533b60e2f654d15368dd712baca3c427f1190b43/SFDC%20Case%20Bookings/Ref%20Join%20Tables/0000%20Plan%20Rules/0000%20Primary%20join%20CaseAttributes%20incl%20Sub%20Territories.md)

```sql

select distinct
a.casenumber
, a.booked_date__c
, case when
    a.booked_date__c = '2020-04-02' then '2021-04-05'::timestamp
    END
    AS booked_date__c_override
, b.name
from salesforce_case AS a
left join "territory"."sheets_join_territory_join sfdc case dm name" as b ON a.dm__c = b.__dm__c
left join "territory"."sheets_join_territory_territories" as c ON b.__sub_territory_id = c.id
where a.finance_sub_status__c = 'Booked'
and c."sub region" = 'Greater China'
and a.booked_date__c IN ('2021-04-02')
```
