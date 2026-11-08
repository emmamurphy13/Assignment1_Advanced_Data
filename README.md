11/7/26

This analysis uses three spreadsheets: 1 Wharton zoning survey, 40 Census building permit files and 3 Census population files.

The spreadsheets come from the following sources:

Wharton Residential Land Use Regulatory Index, 2018 (Gyourko, Hartley & Krimmel): https://real-estate.wharton.upenn.edu/profile/gyourko/
raw_data/wharton/WRLURI_01_15_2020.csv: Raw data of U.S. towns and scores how strictly they regulate land use.
Colums used:
-WRLURI18: The metric of zoning strictness. The score weighs 12 factors. Higher score = more strict.
-cbsatitle18: The metro area of each town.
-GEOID: Census location code
-communityname18: The name of each town.
-state: the name of each state.

U.S. Census Bureau Building Permits Survey: https://www2.census.gov/econ/bps/Place/
raw_data/census/bps/*.txt: Annual building permits by place, 2016–2025
Columns used:
-"year",
-state_code, place_code, cousub_code: combined to use as a location column.
 -months_reported: a number of months that sent the census permit numbers.
-bps_name 
-"units_1", "units_2", "units_3_4", "units_5plus": How many units will be in the building that was permitted.

U.S. Census Bureau Population Estimates: https://www2.census.gov/programs-surveys/popest/datasets/
raw_data/census/sub-est2019_all.csv, sub-est2023.csv, sub-est2025.csv: Raw data of town populations, 2010–2025
Columns used:
STATE, PLACE, COUSUB: Combined to make location code.
POPESTIMATE: Gives an estimated population for each year.


Methodology

01_match_2018_2022.ipynb matches the Wharton zoning survey to Census building permits and population.
- Builds a 7-digit Census location code for each town (state + place code). This is what matches the three data sets.
- Find the average number of apartments built each year between 2018-2022. (Total apartments built 2018-2022 / 5 years).
- Drops towns without a WRLURI18 score, leaving 1,995 towns.
- Calculates permits per 1,000 residents each town granted.

01_match_2021_2025.ipynb matches permits to population for every place in the country, not just Wharton towns.
- Calculates average annual permits for 2016–2020 and 2021–2025.
- Keeps places with at least 3 years of permit data in 2021–2025 and matches them to 2021–2025 population estimates.

02_analyze.ipynb performs the following analyses:
- Sorts towns into "Lightly regulated", "Highly regulated" and "Moderately regulated" based on their WRLURI18 score. (This follows the methodology of the Wharton professors where WRLURI18 scores are split into percentiles. This one is based off of all towns the Wharton professors just use metro areas).
- Uses the census defintion to sort starts into regions.
- Grouped all the matching cbsatitle18 (metro areas) together. If they contained 10 or more towns they stayed in the data frame. (This follows the same metodology as the Wharton professors).
- Applied the strictness scores to the metro areas.(This follows the methodology of the Wharton professors where WRLURI18 scores are split into percentiles. This time the WRLURI18 score quartiles are exactly the same because it's metros.)
- Drop metros that don't have both population and permit data.
- Calculate the population growth of each town between 2010 and 2017. This won't conflict with the year of permits (2018-2022) which is important becuase these two variables affect eachother.
- **Road to Housing analysis** CDBG grants to municipalities will be impacted by the growth of their housing units.
- Filter out places with a population of less than 50,000. This is because CDBG grants are almost always given to cities with populations of 50,000 or more.
- The act compares building in the last 5 years (2021-2025) with the 5 years before (2016-2020). Calculate an improvement score (like the act) with this formula: improvement = (new - old) / (new + old).
Find the median improvement score and create a table.
- While the act will use units built to do these calculations, this data set uses permit data due to accessibility.

Outputs

The notebooks output these spreadsheets:
- analysis/wharton_permits_matched.csv: the 1,995 Wharton towns, with their WRLURI18 score, metro area, 2018–2022 average annual permits, population and permits per 1,000 residents.
- analysis/wharton_permits_matched_2021_2025.csv: every place with 3+ years of 2021–2025 permit data, with average annual permits for 2016–2020 and 2021–2025, 2021–2025 population and permits per 1,000 residents.

Running the analysis yourself

You can run the analysis yourself. To do so, you'll need the following installed on your computer:

Python 3
Python pandas library
Licensing

All code in this repository is available under the MIT License. The data file in the output/ directory is available under the Creative Commons Attribution 4.0 International (CC BY 4.0) license. All files in the data/ directory are released into the public domain.

Feedback / Questions?

Contact Emma Ferrara at emma.ferrara@proton.me.