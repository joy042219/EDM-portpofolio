# Midterm Lab Task 2- Data Cleaning and Transformation using POWER QUERY
 For this task we are given a Dataset that we need it to Cleaned and Transformed it using Power Query

# Screenshot of the Dataset before doing Cleaning and Transformation
 <img src="image/Dataset.png" alt="Alt Text" width="400" height="300">
 
# Steps perform in Data Cleaning and Transformation

let
    Source = Excel.Workbook(File.Contents("C:\Users\Computer LAB\Downloads\Uncleaned_DS_jobs.xlsx"), null, true),
    Uncleaned_DS_jobs_Sheet = Source{[Item="Uncleaned_DS_jobs",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Uncleaned_DS_jobs_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"index", Int64.Type}, {"Job Title", type text}, {"Salary Estimate", type text}, {"Job Description", type text}, {"Rating", type number}, {"Company Name", type text}, {"Location", type text}, {"Headquarters", type any}, {"Size", type any}, {"Founded", Int64.Type}, {"Type of ownership", type any}, {"Industry", type any}, {"Sector", type any}, {"Revenue", type any}, {"Competitors", type any}}),
    #"Extracted Text Before Delimiter" = Table.TransformColumns(#"Changed Type", {{"index", each Text.BeforeDelimiter(Text.From(_, "en-PH"), "("), type text}}),
    #"Inserted Text Between Delimiters" = Table.AddColumn(#"Extracted Text Before Delimiter", "Min Sal", each Text.BetweenDelimiters([Salary Estimate], "$", "K"), type text),
    #"Inserted Text Between Delimiters1" = Table.AddColumn(#"Inserted Text Between Delimiters", "Max Sal", each Text.BetweenDelimiters([Salary Estimate], "$", "K", 1, 0), type text),
    #"Added Custom" = Table.AddColumn(#"Inserted Text Between Delimiters1", "Role Type", each if Text.Contains([Job Title], "Data Scientist") then
"Data Scientist"
else if Text.Contains([Job Title], "Data Analyst") then
"Data Analyst"
else if Text.Contains([Job Title], "Data Engineer") then
"Data Engineer"

else if Text.Contains([Job Title], "Machine Learning") then
"Machine Learning Engineer"
else
"other"),
    #"Changed Type1" = Table.TransformColumnTypes(#"Added Custom",{{"Role Type", type text}}),
    #"Added Custom1" = Table.AddColumn(#"Changed Type1", "Custom", each if [Location]= "New Jersey" then ", NJ"
else if [Location] = "Remote" then ", other"
else if [Location]= "United States" then ", other"
else if [Location]= "Texas" then ", TX"
else if [Location]= "Patuxent" then ", MA"
else if [Location]= "California" then ", CA"
else if [Location]= "Utah" then ", UT"
else [Location]),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Added Custom1", "Custom", Splitter.SplitTextByDelimiter(",", QuoteStyle.Csv), {"Custom.1", "Custom.2"}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Custom.1", type text}, {"Custom.2", type text}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type2",{{"Custom.1", "Location Correction.1"}, {"Custom.2", "Location Correction.2"}}),
    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns","Anne Rundell","MA",Replacer.ReplaceText,{"Location Correction.2"}),
    #"Renamed Columns1" = Table.RenameColumns(#"Replaced Value",{{"Location Correction.2", "State Abbrevations"}}),
    #"Inserted Text Before Delimiter" = Table.AddColumn(#"Renamed Columns1", "MinCompanySize", each Text.BeforeDelimiter([Size], " "), type text),
    #"Inserted Text Between Delimiters2" = Table.AddColumn(#"Inserted Text Before Delimiter", "MaxCompanySize", each Text.BetweenDelimiters([Size], " ", " ", 1, 0), type text),
    #"Filtered Rows" = Table.SelectRows(#"Inserted Text Between Delimiters2", each ([Competitors] <> -1)),
    #"Filtered Rows1" = Table.SelectRows(#"Filtered Rows", each ([Industry] <> -1)),
    #"Inserted Text Before Delimiter1" = Table.AddColumn(#"Filtered Rows1", "Text Before Delimiter", each Text.BeforeDelimiter([Company Name], "#(lf)"), type text),
    #"Removed Columns" = Table.RemoveColumns(#"Inserted Text Before Delimiter1",{"Company Name"}),
    #"Renamed Columns2" = Table.RenameColumns(#"Removed Columns",{{"Text Before Delimiter", "Company Name"}}),
    #"Removed Columns1" = Table.RemoveColumns(#"Renamed Columns2",{"Job Description"})
in
    #"Removed Columns1"
