---
View: sfdc-w003v1-t006-fact-split-cases-union
---
```

select
id:: character varying (200) as id,
casenumber:: character varying (200) as casenumber,
teamvalue:: character varying (200) as teamvalue,
bookingvalue_g as nbvlocaloverride,
allocation,
calculationflag::character varying (200) as calculationflag,
inet_now_licenses__c_grouped,
inet_safer_synergy__c_grouped::character varying (200) as inet_safer_synergy__c_grouped
from
"sfdc-w003v1-t005-fact-split-cases-grouped"

UNION

select
id:: character varying (200) as id,
casenumber:: character varying (200) as casenumber,
teamvalue:: character varying (200) as teamvalue,
bookingvalue as nbvlocaloverride,
allocation,
calculationflag::character varying (200) as calculationflag,
null::integer as inet_now_licenses__c_grouped,
null::character varying (200) as inet_safer_synergy_grouped
from
"sfdc-w003v1-t005-fact-split-cases-non-grouped"
