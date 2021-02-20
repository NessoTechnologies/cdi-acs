![Nesso Technologies Logo](/images/nesso_typemark_375px.png)

# CDI/ACS Data

## Introduction

These files are created by enriching US Chronic Disease Indicators data with American Community Survey Demographic and Housing Estimates:

## Sources

**Primary Dataset**: US Chronic Disease Indicators (CDI) (https://catalog.data.gov/dataset/u-s-chronic-disease-indicators-cdi).  All fields from this dataset are included.  Only a single year is included (where YearStart and YearEnd are the same).  The year is included in the file name. 


**Enrichment Dataset**: American Community Survey (ACS) Demographic and Housing Estimates (https://data.census.gov/cedsci/table?q=United%20States&tid=ACSDP1Y2019.DP05&hidePreview=true).  Only population estimates and margins of error are included from this dataset, and only for the matching gender and race/ethnicity segments from the primary data.  Only data from the same year represented in the primary data are included.


## File Format
Each file consists of two groups of columns:

* The first group contains every column from the primary dataset, US Chronic Disease Indicators (CDI).  For details about those columns and their datatypes, please see the source link listed in the preceding section for that dataset.  Only data for a given year are included (the specific year is denoted in the file name and in the YearStart/YearEnd columns in the file).
* The second group contains four columns that represent data from the American Community Survey (ACS) Demographic and Housing Estimates data:

Field | Datatype | Description
----- | -------- | -----------
Population | Integer | The sum of the population data for the given CDI year, location, and stratification category.  If a given combination of year, location, and stratification category does not exist in the source data, or there are statistical problems with including it, the number will be blank.1
PopulationMarginOfError	| Integer | The aggregate of the margins of error for the association population. This is calculated as the square root of the sum of the squares of the margins of error being aggregated.2  If a given combination of year, location, and stratification category does not exist in the source data, or there are statistical problems with including it, the number will be blank.1

1 For details on why data may be missing, please see the data.census.gov link in the preceding section, and specifically the “Notes” attached to ACS data.

2 This is described on slide 50 of https://www.census.gov/content/dam/Census/programs-surveys/acs/guidance/training-presentations/20180418_MOE.pdf


## Enrichment Logic

