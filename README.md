# Henley Passport Index Data - Tidy Tuesday - 09.09.2025
This week we are exploring data from the Henley Passport Index API. The Henley Passport Index is produced by Henley & Partners and captures the number of countries to which travelers in possession of each passport in the world may enter visa free.

For each travel destination, if no visa is required for passport holders from a country or territory, then a score with value = 1 is created for that passport. A score with value = 1 is also applied if passport holders can obtain a visa on arrival, a visitor’s permit, or an electronic travel authority (ETA) when entering the destination. These visa-types require no pre-departure government approval, because of the specific visa-waiver programs in place. Where a visa is required, or where a passport holder has to obtain a government-approved electronic visa (e-Visa) before departure, a score with value = 0 is assigned. A score with value = 0 is also assigned if passport holders need pre-departure government approval for a visa on arrival, a scenario we do not consider ‘visa-free’. The total score for each passport is equal to the number of destinations for which no visa is required (value = 1), under the conditions defined above.

Henley & Partners update the Global Passport Index rankings each month and changes to the US passport rank captured media attention recently.

Thank you to [Brenden Smith and Jen Richmond](https://github.com/brendensm @jenrichmond) for curating this week's dataset.

## Dataset
* **Source:** [Tidy Tuesday - 09.09.2025](https://github.com/rfordatascience/tidytuesday/blob/main/data/2025/2025-09-09/)
* **Source:** [Oxford Covid-19 Government Response Tracker (OxCGRT)](https://github.com/OxCGRT/covid-policy-dataset/tree/main)
* **Source:** [Worldwide Governance Indicators](https://www.worldbank.org/en/publication/worldwide-governance-indicators)
* **Source:** [World Bank Group - GDP growth (annual %)](https://data.worldbank.org/indicator/NY.GDP.MKTP.KD.ZG)

## Tech Stack
* `pandas`
* `np`
* `power bi`

## Tidy Tuesday questions
- Which countries have made the most dramatic improvements in passport power? Which passports have lost the most visa-free access?
- Do countries with stronger trade relationships tend to offer mutual visa-free access?
- Did the COVID-19 pandemic affect passport rankings, particularly for countries that implemented strict border controls?
- How does political instability or economic crisis impact passport strength?

## Key Steps
1. **Step 1. Loading data**
2. **Step 2. Exploring Dataset**
3. **Step 3. Cleaning Data**
4. **Step 4. Exporting data for Power BI Dashboard**
4. **Step 5. Power BI Dashboard Creation**

## Project Objective
To keep practicing data analytics, dashboard creation pandas and power bi. I will attempt to answer questions stated in this challenge with all the tools available to me. 

In order to answer some of the stated questions i will need to utilise openly available dataset. In particular questions relating to Covid-19 ( for this purpose we will use [Oxford Covid-19 Government Response Tracker (OxCGRT)](https://github.com/OxCGRT/covid-policy-dataset/tree/main)), political instability( for this we will use [Worldwide Governance Indicators](https://www.worldbank.org/en/publication/worldwide-governance-indicators)) and economic crisis impact (For this we will utilise [[World Bank Group - GDP growth (annual %)](https://data.worldbank.org/indicator/NY.GDP.MKTP.KD.ZG)]). 

## Step 2. Exploring Dataset
1. df_country_list

2. df_rank_by_year

3. df_covid_data
This is a very large dataset, most of the data will not be required and i will focus on the data related to the Tidy Tuesday challenge. I will extract data from this dataset: CountryName,CountryCode,Date, C8EV_International travel controls. Record restrictions on international travel. The values stored in this column range from 0 to 4. 0 - no restrictions,1 - screening arrivals , 2 - quarantine arrivals from some or all regions , 3 - ban arrivals from some regions , 4 - ban on all regions or total border closure. 

4. df_gdp_data
This dataset is available publicly. Data comes in a wide format and needs to be transformed in to long format. 

5. df_political_stability_data
Fairly clean and well structured dataset.

## Step 3. Data cleaning
1. df_country list
    * Fill in missing value for Namibia country code as NA
    * Transform original dataframe by exploding records in to separate rows containing country_code, related_country_code, visa_requirements. This will be fact_visa_requirements 
    * Create a dataframe with country names and codes to be used as dimension. This will be dim_countries
    * Crete a dataframe with visa requirements to be used as dimension. This will be dim_requirements
    * Replaced country names with outside ASCII range characters. (Türkiye to Turkey)

2. df_rank_by_year
    * Fill in missing values for Namibia country code
    * Extract unique regions.
    * Merge region in to df_countries dimension
    * Transform region values in to title case
    * Remove region, and country from df_rank_by_year
    * Filled missing values for visa_free_count using forward and backfill

3. df_covid_data
    * Extracted required columns
    * Extracted year data from date integer
    * Corrected country names due to mismatch with df_country
    * Fixed country codes by joining df_country
    * Created dim_travel_controls for travel restrictions
    * Removed duplicate data and monthly granularity by grouping by year, aggregated control score using max control level in a year

5. df_gdp_data
    * Converted from wide to long format to better suit power bi
    * Removed non country entries from the data
    * Fixed naming of countries
    
5. df_political_stability_data
    * Fixed country names
    * Removed unrelated columns
    * Removed string values from estimate
    * Changed estimate data type to float
    * Remove duplicate data by grouping by year, aggregated estimate using mean estimate in a year

4. Save created csv files
    * fact_visa_requirements - stores visa relationships between each countries. 
    * fact_rank - stores country yearly ranking
    * dim_countries - country names / region
    * dim_requirements - visa requirements
    * fact_covid - covid data
    * dim_travel_controls - travel controls dimension

## Dashboard Creation