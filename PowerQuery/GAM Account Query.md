```SQL 
let
    Source = Excel.Workbook(File.Contents("C:\Users\mgrazioso\Fortive\Global Sales - Documents\2 Global files\Reporting\Power BI Reporting\Data Clean Up Location Accounts\DimAccount.xlsx"), null, true),
    //Source = Excel.Workbook(File.Contents("\Dim Location and Account.xlsx"), null, true),
    #"Dim Account_Sheet" = Source{[Item="DimAccount",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(#"Dim Account_Sheet", [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"ACCOUNT NUMBER", type text}, {"ACCOUNT_NAME", type text}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"ACCOUNT_NAME", "ACCOUNT NAME"}}),
    #"Filtered Rows" = Table.SelectRows(#"Renamed Columns", each not Text.Contains( Text.Lower ( [ACCOUNT NAME] ) , "industrial scientific")),
    #"Lowercased Text" = Table.TransformColumns(#"Filtered Rows",{{"ACCOUNT NAME", Text.Lower, type text}}),
   

    #"Added Marencel Column" = Table.AddColumn(#"Lowercased Text", "Bill Marencel", each 
        if Text.Contains([ACCOUNT NAME], "arcelor mittal") then "Cleveland Cliffs" 
        else if Text.Contains([ACCOUNT NAME], "arcelormittal") then "Cleveland Cliffs"
	else if Text.Contains([ACCOUNT NAME], "ak steel") then "Cleveland Cliffs" 
        else if Text.Contains([ACCOUNT NAME], "domtar ") then "Domtar" 
        else if Text.Contains([ACCOUNT NAME], "duke energy") then "Duke Energy" 
        else if Text.Contains([ACCOUNT NAME], "progress ") then "Duke Energy" 
        else if Text.Contains([ACCOUNT NAME], "gerogia pacific") then "Georgia Pacific" 
        else if Text.Contains([ACCOUNT NAME], "international paper") then "International Paper" 
        else if Text.Contains([ACCOUNT NAME], "nucor ") then "Nucor"
        else if Text.Contains([ACCOUNT NAME], "p&g") then "P&G"
        else if Text.Contains([ACCOUNT NAME], "sonoco products") then "Sonoco Products"
        else if Text.Contains([ACCOUNT NAME], "southern company") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "al power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "alabama power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "ga power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "georgia power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "gulf power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "ms power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "mississippi power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "southern power") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "southern nuclear") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "southern lng") then "Southern Company"
        else if Text.Contains([ACCOUNT NAME], "us steel") then "US Steel"
        else if Text.Contains([ACCOUNT NAME], "union railroad") then "US Steel"
        else if Text.Contains([ACCOUNT NAME], "veolia") then "Veolia"
        else if Text.Contains([ACCOUNT NAME], "westrock") then "WestRock"
        else if Text.Contains([ACCOUNT NAME], "meadwestvaco") then "WestRock"
        else if Text.Contains([ACCOUNT NAME], "rocktenn") then "WestRock"
        else if Text.Contains([ACCOUNT NAME], "smurfit stone") then "WestRock"
        else 0),


    #"Added Thomas Column" = Table.AddColumn(#"Added Marencel Column", "Bill Thomas", each 
        if Text.Contains([ACCOUNT NAME], "exxonmobil") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "exxon") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "xto ") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "esso ") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "syncrude ") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "imperial oil") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "infineum ") then "ExxonMobil" 
        else if Text.Contains([ACCOUNT NAME], "trend gathering") then "ExxonMobil"
	else if Text.Contains([ACCOUNT NAME], "noble energy") then "Chevron" 
       
       //Tie Breaker for Chevron and Chevron Phillips Chemical between BT and TH
        else if Text.Contains([ACCOUNT NAME], "chevron phillips chemical") then 0 
        else if Text.Contains([ACCOUNT NAME], "chevron") then "Chevron" 
        
        else if Text.Contains([ACCOUNT NAME], "koch ") then "Koch" 
        else if Text.Contains([ACCOUNT NAME], "flint hills") then "Koch" 
        else if Text.Contains([ACCOUNT NAME], "marathon oil") then "Marathon Oil"
        else if Text.Contains([ACCOUNT NAME], "marathon petroleum") then "Marathon Petroleum" 
        else if Text.Contains([ACCOUNT NAME], "andeavor ") then "Marathon Petroleum" 
        else if Text.Contains([ACCOUNT NAME], "tesoro ") then "Marathon Petroleum" 
        else if Text.Contains([ACCOUNT NAME], "western refining") then "Marathon Petroleum" 
        else if Text.Contains([ACCOUNT NAME], "markwest ") then "Marathon Petroleum"
        else if Text.Contains([ACCOUNT NAME], "motiva ") then "Motiva" 
        else 0),
    
    
    
    #"Added Holiman Column" = Table.AddColumn(#"Added Thomas Column", "Troy Holiman", each 
        if Text.Contains([ACCOUNT NAME], "basf ") then "BASF" 
        else if Text.Contains([ACCOUNT NAME], "wintershall") then "BASF" 
        else if Text.Contains([ACCOUNT NAME], "cognis ") then "BASF" 
        else if Text.Contains([ACCOUNT NAME], "engelhard") then "BASF" 
        else if Text.Contains([ACCOUNT NAME], "synvina ") then "BASF"
	else if Text.Contains([ACCOUNT NAME], "chevron phillips chemical") then "Chevron Phillips Chemical" 
        else if Text.Contains([ACCOUNT NAME], "chevron phillips") then "Chevron Phillips Chemical" 
        else if Text.Contains([ACCOUNT NAME], "american styrenics") then "Chevron Phillips Chemical" 
        else if Text.Contains([ACCOUNT NAME], "drilling specialties") then "Chevron Phillips Chemical" 
        else if Text.Contains([ACCOUNT NAME], "performance pipe") then "Chevron Phillips Chemical"
        else if Text.Contains([ACCOUNT NAME], "chemours ") then "Chemours" 
        else if Text.Contains([ACCOUNT NAME], "first chemical") then "Chemours"
	else if Text.Contains([ACCOUNT NAME], "citgo") then "Citgo" 
        else if Text.Contains([ACCOUNT NAME], "dow ") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "dupont ") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "corteva ") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "danisco ") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "dow corning") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "genencor ") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "mycogen ") then "Dow DuPont Corteva" 
       
       //Tie Breaker for Pioneer and Pioneer Natural Resources between TH and BT
        else if Text.Contains([ACCOUNT NAME], "pioneer natural resource") then 0
        else if Text.Contains([ACCOUNT NAME], "pioneer ") then "Dow DuPont Corteva" 
        
        else if Text.Contains([ACCOUNT NAME], "pioneer hi breed") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "rohm & haas") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "spruance plant") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "solae ") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "union carbide") then "Dow DuPont Corteva" 
        else if Text.Contains([ACCOUNT NAME], "eastman chemical") then "Eastman Chemical" 
        else if Text.Contains([ACCOUNT NAME], "mustang pipeline") then "Eastman Chemical" 
        else if Text.Contains([ACCOUNT NAME], "solutia ") then "Eastman Chemical" 
        else if Text.Contains([ACCOUNT NAME], "taminco ") then "Eastman Chemical" 
        else if Text.Contains([ACCOUNT NAME], "olin ") then "Olin" 
        else if Text.Contains([ACCOUNT NAME], "blue cube") then "Olin" 
        else if Text.Contains([ACCOUNT NAME], "bridgeport brass") then "Olin" 
        else if Text.Contains([ACCOUNT NAME], "k.a. steel chemicals") then "Olin" 
        else if Text.Contains([ACCOUNT NAME], "winchester ") then "Olin" 
        else 0),
  
  
  
    #"Added Bourque Column" = Table.AddColumn(#"Added Mork Davis Column", "Bobby Bourque", each 
        if Text.Contains([ACCOUNT NAME], "bp ") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "air bp") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "aker bp") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "bp canada") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "bp shipping") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "castrol ") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "olympic pipeline") then "BP" 
        else if Text.Contains([ACCOUNT NAME], "clean harbors") then "Clean Harbors"
        else if Text.Contains([ACCOUNT NAME], "safety-kleen") then "Clean Harbors" 
        else if Text.Contains([ACCOUNT NAME], "conocophillips") then "ConocoPhillips" 
        else if Text.Contains([ACCOUNT NAME], "burilington resources") then "ConocoPhillips" 
        else if Text.Contains([ACCOUNT NAME], "conoco ") then "ConocoPhillips" 
        else if Text.Contains([ACCOUNT NAME], "continental oil company") then "ConocoPhillips" 
        else if Text.Contains([ACCOUNT NAME], "enbridge ") then "Enbridge" 
        else if Text.Contains([ACCOUNT NAME], "spectra ") then "Enbridge" 
        else if Text.Contains([ACCOUNT NAME], "midcoast energy partners") then "Enbridge" 
        else if Text.Contains([ACCOUNT NAME], "magellan midstream") then "Magellan Midstream" 
        else if Text.Contains([ACCOUNT NAME], "marerro terminal") then "Magellan Midstream" 
        else if Text.Contains([ACCOUNT NAME], "magellan pipeline") then "Magellan Midstream" 
        else if Text.Contains([ACCOUNT NAME], "bridgetex") then "Magellan Midstream" 
        else if Text.Contains([ACCOUNT NAME], "nustar ") then "NuStar" 
        else if Text.Contains([ACCOUNT NAME], "diamond k") then "NuStar" 
        else if Text.Contains([ACCOUNT NAME], "legacystar") then "NuStar" 
        else if Text.Contains([ACCOUNT NAME], "s t linden terminal") then "NuStar" 
        else if Text.Contains([ACCOUNT NAME], "shore terminals ") then "NuStar" 
        else if Text.Contains([ACCOUNT NAME], "oxy chemicals") then "Oxy Chemicals" 
        else if Text.Contains([ACCOUNT NAME], "oxy oil & gas") then "Oxy Oil & Gas" 
        else if Text.Contains([ACCOUNT NAME], "oxy oil and gas") then "Oxy Oil & Gas" 
        else if Text.Contains([ACCOUNT NAME], "anadarko ") then "Oxy Oil & Gas" 
        else if Text.Contains([ACCOUNT NAME], "centurian pipeline") then "Oxy Oil & Gas" 
        else if Text.Contains([ACCOUNT NAME], "vintage petroleum") then "Oxy Oil & Gas" 
        else if Text.Contains([ACCOUNT NAME], "pbf energy") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "torrance refining") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "de city refining") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "toledo refining") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "jones lang lasalle") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "paulsboro refining") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "crowne point") then "PBF Energy" 
        else if Text.Contains([ACCOUNT NAME], "phillips 66") then "Phillips 66" 
        else if Text.Contains([ACCOUNT NAME], "jet patrol") then "Phillips 66" 
        else if Text.Contains([ACCOUNT NAME], "phillips gas company") then "Phillips 66" 
        else if Text.Contains([ACCOUNT NAME], "dcp midstream") then "Phillips 66" 
        else if Text.Contains([ACCOUNT NAME], "valero ") then "Valero" 
        else if Text.Contains([ACCOUNT NAME], "ultramar ") then "Valero" 
        else if Text.Contains([ACCOUNT NAME], "diamond shamrock") then "Valero"
	else if Text.Contains([ACCOUNT NAME], "enterprise products") then "Enterprise Products" 
        else if Text.Contains([ACCOUNT NAME], "oiltanking") then "Enterprise Products" 
        else if Text.Contains([ACCOUNT NAME], "dixie pipeline") then "Enterprise Products" 
        else if Text.Contains([ACCOUNT NAME], "mid-america pipeline") then "Enterprise Products"
	else if Text.Contains([ACCOUNT NAME], "shell ") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "convent ") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "norco ") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "aera energy") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "equilon enterprises") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "criterion catalysts") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "pecten chemicals") then "Shell" 
        else if Text.Contains([ACCOUNT NAME], "pennzoil") then "Shell"
        else 0),
  
  
    #"Added Matson Column" = Table.AddColumn(#"Added Bourque Column", "Eric Matson", each 
        if Text.Contains([ACCOUNT NAME], "suncor ") then "Suncor Energy" 
        else if Text.Contains([ACCOUNT NAME], "oilsands refining") then "Suncor Energy" 
        else if Text.Contains([ACCOUNT NAME], "husky energy") then "Cenovus Energy" 
        else if Text.Contains([ACCOUNT NAME], "cenovus ") then "Cenovus Energy" 
        else if Text.Contains([ACCOUNT NAME], "canadian natural resource") then "Canadian Natural Resources" 
        else if Text.Contains([ACCOUNT NAME], "cnrl ") then "Canadian Natural Resources" 
        else if Text.Contains([ACCOUNT NAME], "north american natural gas") then "Canadian Natural Resources" 
        else if Text.Contains([ACCOUNT NAME], "inter pipeline") then "Inter Pipeline" 
        else if Text.Contains([ACCOUNT NAME], "heartland petrochemical") then "Inter Pipeline" 
        else if Text.Contains([ACCOUNT NAME], "ngl processing") then "Inter Pipeline" 
        else if Text.Contains([ACCOUNT NAME], "pembina pipeline") then "Pembina Pipeline"
        else if Text.Contains([ACCOUNT NAME], "teck resources") then "Teck Resources"
	else if Text.Contains([ACCOUNT NAME], "tech coal") then "Teck Resources"
	else if Text.Contains([ACCOUNT NAME], "tech metals") then "Teck Resources"
	else if Text.Contains([ACCOUNT NAME], "tech water") then "Teck Resources"
	else if Text.Contains([ACCOUNT NAME], "sasol") then "Sasol"
	else if Text.Contains([ACCOUNT NAME], "stantec") then "Stantec"
	else if Text.Contains([ACCOUNT NAME], "arc resource") then "ARC Resources"
        
        //Tie Breaker for VALE and Valero between EM and BB
        else if Text.Contains([ACCOUNT NAME], "valero ") then 0
        else if Text.Contains([ACCOUNT NAME], "vale ") then "VALE" 
        else 0),
   
   
    #"Unpivoted Other Columns" = Table.UnpivotOtherColumns(#"Added Matson Column", {"ACCOUNT NAME", "ACCOUNT NUMBER"}, "Attribute", "Value"),
   
    #"Filtered Rows1" = Table.SelectRows(#"Unpivoted Other Columns", each [Value] <> 0),
   
    #"Sorted Rows" = Table.Sort(#"Filtered Rows1",{{"ACCOUNT NUMBER", Order.Ascending}})
in
    #"Sorted Rows"
