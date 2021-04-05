```pq
let
  Source = #"ref Case Bookings dedup",
  #"Filtered rows: Booked Finance Sub Status" = Table.SelectRows(Source, each [Finance_Sub_Status__c] = "Booked"),
  #"Replaced value 7" = Table.ReplaceValue(#"Filtered rows: Booked Finance Sub Status", null, false, Replacer.ReplaceValue, {"Billing_Agent__c"}),
  #"Replaced value 4" = Table.ReplaceValue(#"Replaced value 7", null, 0, Replacer.ReplaceValue, {"Current_Monthly_Subscription_Fee__c", "Previous_Monthly_Subscription_Fee__c", "Previous_MRR_USD__c", "Current_MRR_USD__c", "MRR_Change_USD__c", "iNet_Now_Licenses__c"}),
  #"Clean up operation: Contract Length" = Table.TransformColumnTypes(#"Replaced value 4", {{"Contract_Length__c", type text}}),
  #"Replaced value 1" = Table.ReplaceValue(#"Clean up operation: Contract Length", "0", "1", Replacer.ReplaceValue, {"Contract_Length__c"}),
  #"Replaced value 2" = Table.ReplaceValue(#"Replaced value 1", "", "1", Replacer.ReplaceValue, {"Contract_Length__c"}),
  #"Replaced value 3" = Table.ReplaceValue(#"Replaced value 2", null, "1", Replacer.ReplaceValue, {"Contract_Length__c"}),
  #"Changed column type 12" = Table.TransformColumnTypes(#"Replaced value 3", {{"Contract_Length__c", Int64.Type}}),
  #"Removed duplicates" = Table.Distinct(#"Changed column type 12", {"Id"}),
  #"Added custom: Data Last Refreshed GMT" = Table.AddColumn(#"Removed duplicates", "Data Last Refreshed GMT", each DateTimeZone.FixedUtcNow()),
  #"Changed column type" = Table.TransformColumnTypes(#"Added custom: Data Last Refreshed GMT", {{"Data Last Refreshed GMT", type text}}),
  #"Changed column type 1" = Table.TransformColumnTypes(#"Changed column type", {{"Data Last Refreshed GMT", type datetimezone}}),
  #"Divided column" = Table.TransformColumns(#"Changed column type 1", {{"SPIFF_Commission__c", each _ / 100, type number}}),
  #"Changed column type 4" = Table.TransformColumnTypes(#"Divided column", {{"SPIFF_Commission__c", Percentage.Type}}),
  #"Changed column type 2" = Table.TransformColumnTypes(#"Changed column type 4", {{"Data Last Refreshed GMT", type datetime}}),
  #"Divided column 1" = Table.TransformColumns(#"Changed column type 2", {{"Distributor_Commission__c", each _ / 100, type number}}),
  #"Changed column type 3" = Table.TransformColumnTypes(#"Divided column 1", {{"Distributor_Commission__c", Percentage.Type}}),
  #"Inserted conditional column: Distributor Commission %" = Table.AddColumn(#"Changed column type 3", "Distributor Commission %", each if [Billing_Agent__c] <> true then [Distributor_Commission__c] else if [Billing_Agent__c] = true then 0 else [Distributor_Commission__c]),
  #"Replaced value 5" = Table.ReplaceValue(#"Inserted conditional column: Distributor Commission %", null, 0, Replacer.ReplaceValue, {"Distributor_Commission__c"}),
  #"Changed column type 5" = Table.TransformColumnTypes(#"Replaced value 5", {{"Distributor Commission %", Percentage.Type}}),
  #"Replaced value 6" = Table.ReplaceValue(#"Changed column type 5", null, 0, Replacer.ReplaceValue, {"Distributor Commission %"}),
  #"Changed column type 13" = Table.TransformColumnTypes(#"Replaced value 6", {{"Distributor_Commission__c", Percentage.Type}, {"Distributor Commission %", Percentage.Type}}),
  #"Inserted conditional column 1" = Table.AddColumn(#"Changed column type 13", "Billing Agent Discount %", each if [Billing_Agent__c] = true then [Distributor_Commission__c] else 0),
  #"Changed column type 6" = Table.TransformColumnTypes(#"Inserted conditional column 1", {{"Billing Agent Discount %", Percentage.Type}}),
  #"Replaced value" = Table.ReplaceValue(#"Changed column type 6", null, 0, Replacer.ReplaceValue, {"Billing Agent Discount %", "Distributor Commission %", "SPIFF_Commission__c"}),
  #"Changed column type 7" = Table.TransformColumnTypes(#"Replaced value", {{"Billing Agent Discount %", Percentage.Type}, {"Distributor Commission %", Percentage.Type}, {"SPIFF_Commission__c", Percentage.Type}}),
  #"Added custom TCV Local (NBV less Billing Agent Discount)" = Table.AddColumn(#"Changed column type 7", "TCV Booking Local", each [Net_Bookings_Value__c] * ( 1 - [#"Billing Agent Discount %"] )),
  #"Changed column type 15" = Table.TransformColumnTypes(#"Added custom TCV Local (NBV less Billing Agent Discount)", {{"TCV Booking Local", type number}}),
  #"Added custom ACV Local" = Table.AddColumn(#"Changed column type 15", "ACV Booking Local", each if [Contract_Length__c] < 13 then
    [TCV Booking Local]
    else
    Value.Divide( [TCV Booking Local]*12,[Contract_Length__c])),
  #"Changed column type 8" = Table.TransformColumnTypes(#"Added custom ACV Local", {{"ACV Booking Local", type number}}),
  #"Added custom ACV Expansion: MRR Change Local  >0 and NBV >0" = Table.AddColumn(#"Changed column type 8", "ACV Booking Expansion+ Local", each if ([Current_Monthly_Subscription_Fee__c]-[Previous_Monthly_Subscription_Fee__c]) > 0 and [Contract_Length__c] < 13 and [Net_Bookings_Value__c] > 0
    then
    ([Current_Monthly_Subscription_Fee__c]-[Previous_Monthly_Subscription_Fee__c]) * [Contract_Length__c]
    else if ([Current_Monthly_Subscription_Fee__c]-[Previous_Monthly_Subscription_Fee__c]) > 0 and [Net_Bookings_Value__c] > 0 and [Contract_Length__c] > 12
    then
    ([Current_Monthly_Subscription_Fee__c]-[Previous_Monthly_Subscription_Fee__c]) * 12
    else
    0),
  #"Changed column type 10" = Table.TransformColumnTypes(#"Added custom ACV Expansion: MRR Change Local  >0 and NBV >0", {{"ACV Booking Expansion+ Local", type number}}),
  // ACV Override Correction
  #"Added custom ACV Local Expansion+ override corrections" = Table.AddColumn(#"Changed column type 10", "ACV Booking Expansion+ Local override", each if [#"ACV Booking Expansion+ Local"] > [ACV Booking Local] and [ACV Booking Local] > 0
    then [ACV Booking Local]
    else if [ACV Booking Local] < 0
    then 0
    else
    [#"ACV Booking Expansion+ Local"]),
  #"Changed column type 14" = Table.TransformColumnTypes(#"Added custom ACV Local Expansion+ override corrections", {{"ACV Booking Expansion+ Local override", type number}}),
  #"Added ACV Non Expansion+ Local" = Table.AddColumn(#"Changed column type 14", "ACV Non Expansion+ Local", each [ACV Booking Local] - [#"ACV Booking Expansion+ Local override"], Currency.Type),
  #"Changed column type 11" = Table.TransformColumnTypes(#"Added ACV Non Expansion+ Local", {{"ACV Non Expansion+ Local", type number}}),
  #"Removed columns" = Table.RemoveColumns(#"Changed column type 11", {"ACV Booking Expansion+ Local"}),
  #"Renamed columns" = Table.RenameColumns(#"Removed columns", {{"ACV Booking Expansion+ Local override", "ACV Booking Expansion+ Local"}}),
  #"Added custom: Net Booking Value USD Annualized" = Table.AddColumn(#"Renamed columns", "Net Booking Value USD Annualized", each if [Contract_Length__c] < 13
    then
    [Net_Bookings_Value_USD__c]
    else
    ([Net_Bookings_Value_USD__c]/[Contract_Length__c])*12),
  #"Changed column type 9" = Table.TransformColumnTypes(#"Added custom: Net Booking Value USD Annualized", {{"Net Booking Value USD Annualized", Currency.Type}})
in
  #"Changed column type 9"

```
