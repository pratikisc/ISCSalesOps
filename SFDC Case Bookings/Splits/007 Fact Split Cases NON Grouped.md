---
View: sfdc-w003-t005-fact-split-cases-non-grouped
---

```
select
a.id,
a.casenumber,
a.teamname,
a.bookingvalue,
'Split / Case Value' as calculationflag
from
"sfdc-w003-t004-unpivoted-key-values" a
left join salesforce_case b ON b.casenumber = a.casenumber
WHERE
    
    
    -- !!!!
    -- Remember to check this same list in the WITH ( CaseList ) table in `003 a Final Case Numbers.md`. This is the inverse of Case List.
    (
    b.type not in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
    b.finance_sub_status__c = 'Booked'
    )
    or
    (
    b.type in ('Renewal', 'Amendment', 'Transfer – Acquirer', 'Transfer – Acquiree', 'Term Extension') and
    b.finance_sub_status__c = 'Booked' and
    b.opportunity__c is null
    )