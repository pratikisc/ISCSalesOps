---
title: Transformation (Adding a new key column)
description: For later joins
Status: Interim View
---

```sql


SELECT
    "sfdc-w003-t002-bookingvalues".casenumber || '__S' || right("sfdc-w003-t002-bookingvalues".valueorder, 1) AS id,
    "sfdc-w003-t002-bookingvalues".casenumber,
    "sfdc-w003-t002-bookingvalues".bookingvalue
FROM
    "sfdc-w003-t002-bookingvalues";
