# LAPD-Incident-Report-Dashboard

### Data Source Link : data.gov : https://catalog.data.gov/organization/city-of-los-angeles

## Problem Statement

The Los Angeles Police Department (LAPD) is tasked with maintaining public safety and law enforcement within the city of Los Angeles. As part of its operations, the LAPD needs to efficiently and accurately document incidents, crimes, and other law enforcement activities.

Currently, the LAPD relies on a paper-based or outdated digital incident reporting system, which can be inefficient, error-prone, and time-consuming. There is a need for a modernized Incident Report System that streamlines the process of reporting, recording, and managing incidents, while ensuring data accuracy, security, and accessibility.


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the columns errors & empty values were present except column named "Arrival Delay".
- Step 5 : For calculating average Number of Incidents , null values were not taken into account as only less than 1% values are null in this column(i.e column named "Incident Location") 
- Step 6 : In the report view, under the view tab, theme was selected.
- Step 7 : Since the data contains various Years, thus in order to represent Range, a new visual was added using the Slicer with Range in the visualizations pane in report view. 
- Step 8 : Visual filters (Slicers) were added for four fields named "Year", "Call Type", "Location" & "Dispatch Time".
- Step 9 : Map visualization was used to represent the total number of Incidents per country 
           Using visual level filter from the filters pane, basic filtering was used & null values were unselected for consideration into average calculation.
           
           Although, by default, while calculating average, blank values are ignored.
- Step 10 : A bar chart was also added to the report design area representing the number of Incidents per Year While creating this visual, field named "TotalIncidents" was also added to the Legends bucket, thus number of incidents are also calculated by DAX. 
- Step 11 : Measures were created to calculate : "Average Number of incidents","Common Time Of Incidents", "Most common Incidents","Most occuring Call Type"
  
  
In our dataset, Some parameters were assigned value 0, representing those parameters are not applicable for some incidents.

All these values have been ignored while calculating average rating for each of the parameters mentioned above.

- Step 12 : Calculated column was created in which, incidents were grouped into various Location,call Type groups.

- Step 13 : Date of Incidents was created using DAX

for creating Date Dimension following DAX expression was written;
  Date =
 
//************** Calendar Dimension developed by Hlamulo masonta
 
var _fromYear=2019 // setting the start year of the date dimension.
 
var _toYear=2021   // setting the end year of the date dimension.
 
var _startOfFiscalYear=7 // Assuming that the fiscal starts in July i am setting the start of fiscal year as July (7th month)
 
//**************
 
var _today=TODAY()
 
return
 
ADDCOLUMNS(
 
    CALENDAR(
 
                DATE(_fromYear,1,1),
 
                DATE(_toYear,12,31)
 
),
 
"Year",YEAR([Date]),
 
"Start of Year",DATE( YEAR([Date]),1,1),
 
"End of Year",DATE( YEAR([Date]),12,31),
 
"Month",MONTH([Date]),
 
"Start of Month",DATE( YEAR([Date]), MONTH([Date]), 1),
 
"End of Month",EOMONTH([Date],0),
 
"Days in Month",DATEDIFF(DATE( YEAR([Date]), MONTH([Date]), 1),EOMONTH([Date],0),DAY)+1,
 
"Year Month Number",INT(FORMAT([Date],"YYYYMM")),
 
"Year Month Name",FORMAT([Date],"YYYY-MMM"),
 
"Day",DAY([Date]),
 
"Day Name",FORMAT([Date],"DDDD"),
 
"Day Name Short",FORMAT([Date],"DDD"),
 
"Day of Week",WEEKDAY([Date]),
 
"Day of Year",DATEDIFF(DATE( YEAR([Date]), 1, 1),[Date],DAY)+1,
 
"Month Name",FORMAT([Date],"MMMM"),
 
"Month Name Short",FORMAT([Date],"MMM"),
 
"Quarter",QUARTER([Date]),
 
"Quarter Name","Q"&FORMAT([Date],"Q"),
 
"Year Quarter Number",INT(FORMAT([Date],"YYYYQ")),
 
"Year Quarter Name",FORMAT([Date],"YYYY")&" Q"&FORMAT([Date],"Q"),
 
"Start of Quarter",DATE( YEAR([Date]), (QUARTER([Date])*3)-2, 1),
 
"End of Quarter",EOMONTH(DATE( YEAR([Date]), QUARTER([Date])*3, 1),0),
 
"Week of Year",WEEKNUM([Date]),
 
"Start of Week", [Date]-WEEKDAY([Date])+1,
 
"End of Week",[Date]+7-WEEKDAY([Date]),
 
"Fiscal Year",if(_startOfFiscalYear=1,YEAR([Date]),YEAR([Date])+ QUOTIENT(MONTH([Date])+ (13-_startOfFiscalYear),13)),
 
"Fiscal Quarter",QUARTER( DATE( YEAR([Date]),MOD( MONTH([Date])+ (13-_startOfFiscalYear) -1 ,12) +1,1) ),
 
"Fiscal Month",MOD( MONTH([Date])+ (13-_startOfFiscalYear) -1 ,12) +1,
 
"Day Offset",DATEDIFF(_today,[Date],DAY),
 
"Month Offset",DATEDIFF(_today,[Date],MONTH),
 
"Quarter Offset",DATEDIFF(_today,[Date],QUARTER),
 
"Year Offset",DATEDIFF(_today,[Date],YEAR)
 
)
 
 - Step 17 :My Model was Based on Star Schema
 - Step 18 : The Schema constist of the following:
        Fact 	:LAPD Incidents
	Dimensions	:Date
				 Time
				 Location
				 Incident Type
