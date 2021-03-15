---
---
```

select
id:: character varying (200) as id,
casenumber:: character varying (200) as casenumber,
teamname:: character varying (200) as teamname,
bookingvalue_g as nbvlocaloverride,
allocation,
calculationflag::character varying (200) as calculationflag
from
"sfdc-w003-t005-fact-split-cases-grouped"

UNION

select
id:: character varying (200) as id,
casenumber:: character varying (200) as casenumber,
teamname:: character varying (200) as teamname,
bookingvalue as nbvlocaloverride,
allocation,
calculationflag::character varying (200) as calculationflag
from
"sfdc-w003-t005-fact-split-cases-non-grouped"
