---
title: Initial Load of a flat split table
View Name: sfdc-w003v2-t001-splittable
Status: Interim View
---

```sql
 

               SELECT
                  casenumber,
                  team_territory_assignment_1__c AS team1,
                  team_1_net_booking_value_case_currency__c AS value1,
                  team_territory_assignment_2__c AS team2,
                  team_2_net_booking_value_case_currency__c AS value2,
                  team_territory_assignment_3__c AS team3,
                  team_3_net_booking_value_case_currency__c AS value3,
                  team_territory_assignment_4__c AS team4,
                  team_4_net_booking_value_case_currency__c AS value4,
                  team_territory_assignment_5__c AS team5,
                  team_5_net_booking_value_case_currency__c AS value5,
                  team_territory_assignment_6__c AS team6,
                  team_6_net_booking_value_case_currency__c AS value6,
                  team_territory_assignment_7__c AS team7,
                  team_7_net_booking_value_case_currency__c AS value7,
                  team_territory_assignment_8__c AS team8,
                  team_8_net_booking_value_case_currency__c AS value8
              FROM
                  salesforce_case
              WHERE
                 team_territory_assignment_1__c IS NOT NULL
         
         
        
 
 


```

## View Name: 

| Column | Description |
| --- | --- |
|`All Column Descriptions`| All column descriptions are in the final view  |

