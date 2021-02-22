# US Chronic Disease Indicators data enriched with American Community Survey Demographic and Housing Estimates

## Introduction

These files are created by enriching US Chronic Disease Indicators (CDI) data with American Community Survey Demographic and Housing Estimates (ACS).  You'll find ZIPed CSVs in the [`data/`](https://github.com/NessoTechnologies/cdi-acs/tree/main/data) directory, one for each year.

These files are maintained by [Nesso Technologies, Inc.](https://nesso.io).  If you have any questions or issues with these files, please [create a GitHub issue](https://github.com/NessoTechnologies/cdi-acs/issues).  We'd love to hear from you!

## Sources

**Primary Dataset**: [US Chronic Disease Indicators (CDI)](https://catalog.data.gov/dataset/u-s-chronic-disease-indicators-cdi).  All fields from this dataset are included.  Only a single year is included (where YearStart and YearEnd are the same).  The year is included in the file name.  This dataset is sourced from the CDC, and is in the public domain.


**Enrichment Dataset**: [American Community Survey (ACS) Demographic and Housing Estimates](https://data.census.gov/cedsci/table?q=United%20States&tid=ACSDP1Y2019.DP05&hidePreview=true).  Only population estimates and margins of error are included from this dataset, and only for the matching gender and race/ethnicity segments from the primary data.  Only data from the same year represented in the primary data are included.  This dataset is sourced from the Census Bureau, and is in the public domain.



## File Format
Each CDI/ACS file consists of two groups of columns:

* The first group contains every column from the primary dataset, US Chronic Disease Indicators (CDI).  For details about those columns and their datatypes, please see the source link listed in the preceding section for that dataset.  Only data for a given year are included (the specific year is denoted in the file name and in the YearStart/YearEnd columns in the file).
* The second group contains four columns that represent data from the American Community Survey (ACS) Demographic and Housing Estimates data:

Field | Datatype | Description
----- | -------- | -----------
Population | Integer | The sum of the population data for the given CDI year, location, and stratification category.  If a given combination of year, location, and stratification category does not exist in the source data, or there are statistical problems with including it, the number will be blank.<sup>1</sup>
PopulationMarginOfError	| Integer | The aggregate of the margins of error for the association population. This is calculated as the square root of the sum of the squares of the margins of error being aggregated.<sup>2</sup>  If a given combination of year, location, and stratification category does not exist in the source data, or there are statistical problems with including it, the number will be blank.<sup>1</sup>

<sup>1</sup> For details on why data may be missing, please see the "Notes" attached to the [ACS data](https://data.census.gov/cedsci/table?q=United%20States&tid=ACSDP1Y2019.DP05&hidePreview=true).

<sup>2</sup> This is described on slide 50 of [this Census.gov slidedeck](https://www.census.gov/content/dam/Census/programs-surveys/acs/guidance/training-presentations/20180418_MOE.pdf).


## Enrichment Logic
Each record in the CDI/ACS data is created by joining an ACS file to a CDI file using a combination of Year, Location, and Stratification Category.

### Year
The CDI data is filtered to just those records with the same YearStart and YearEnd, and then joins to the data in ACS with the same Year.

### Location
The CDI data represents location using a pair of fields, LocationAbbr and LocationDesc.  For the most part, these refer to US states/territories.  The ACS data also represents location as state/territory in the NAME column.

The CDI data also contains a “US/United States” location.  For this, the ACS data is aggregated across all states/territories.

### Stratification Category
The CDI data are further broken up into several gender and race/ethnicity categories and values.  These are represented in the StratificationCategory1 and Stratification1 fields.  The ACS data also break out several gender and race/ethnicity segments.  The dataset relates ACS data to CDI data using the following mapping:

CDI Segment | ACS Estimate | ACS Margin of Error
----------- | ------------ | -------------------
Gender: Male | DP05_0002E | DP05_0002M
Gender: Female | DP05_0003E | DP05_0003M
Race/Ethnicity: Multiracial, non-Hispanic | DP05_0035E | DP05_0035M
Race/Ethnicity: American Indian or Alaska Native | DP05_0039E | DP05_0039M
Race/Ethnicity: Asian, non-Hispanic | DP05_0044E | DP05_0044M
Race/Ethnicity: Asian or Pacific Islander | Sum of DP05_0044E and DP05_0052E | Aggregate of DP05_0044M and DP05_0052M
Race/Ethnicity: Black, non-Hispanic | DP05_0038E | DP05_0038M
Race/Ethnicity: Other, non-Hispanic | DP05_0057E | DP05_0057M
Race/Ethnicity: White, non-Hispanic | DP05_0037E | DP05_0037M
Race/Ethnicity: Hispanic | DP05_0071E | DP05_0071M
Overall: Overall | Sum of DP05_0002E and DP05_0003E | Aggregate of DP05_0002M and DP05_0003M



