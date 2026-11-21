Household Electricity Consumption Analysis

Domain: Electricity Consumption (Energy and Utilities) Tools: Python, pandas, NumPy, Matplotlib, Seaborn, VS Code, GitHub Author: Shubh Sharma | Admission No: 2026b01010641 | CSE13 Repository: https://github.com/sharmashubh0908/HCL_PROJECT

Business Problem
A regional electricity distribution company sees that monthly electricity use differs widely from one household to another, but it does not know which factors cause the difference. The manager does not know how strongly air-conditioner use and home size push consumption up, or whether city, property type, tariff plan and rooftop solar make a difference.

This analysis helps the company to plan electricity supply, design tariffs, and find high-usage households that can be offered energy-saving advice or solar schemes.

The Four Questions
What does Monthly_Consumption_kWh look like overall (typical value, spread and extremes)?

Do AC_Hours_Per_Day and Home_Area_Sqft affect Monthly_Consumption_kWh?

Does Monthly_Consumption_kWh differ by City, Property_Type, Tariff_Plan and Has_Solar?

Which households need attention? A household is labelled High Usage when Monthly_Consumption_kWh is above 350 kWh.

Dataset Item Value File electricity_data.csv (in data/raw) Rows / columns (raw) 255 rows, 12 columns Rows / columns (after cleaning) 250 rows, 12 columns (15 after adding the new columns) Outcome column Monthly_Consumption_kWh Source Synthetic data created with an AI tool (Claude by Anthropic)
Each row is the electricity use of one household for one billing month.

The dataset has no names, phone numbers or e-mail addresses.
Columns: Household_ID, City, Property_Type, Tariff_Plan, Has_Solar, Household_Size, Home_Area_Sqft, AC_Hours_Per_Day, Avg_Temperature_C, Appliance_Age_Years, Reading_Date, Monthly_Consumption_kWh.

Data Cleaning Column Problem found How many How it was fixed All columns Duplicate rows 5 Removed with drop_duplicates() Home_Area_Sqft Missing values 5 Filled with the median of the same Property_Type Tariff_Plan Missing values 4 Filled with the most common value (Standard) Avg_Temperature_C Missing values 5 Filled with the median Appliance_Age_Years Missing values 6 Filled with the median City Inconsistent text (spaces, upper/lower case) 7 Extra spaces removed, spelling made the same AC_Hours_Per_Day Impossible values (above 24 or negative) 2 Replaced with the median
After cleaning: 250 rows, no missing values, no duplicate rows.

New Columns Column How it is calculated Usage_Per_Sqft Monthly_Consumption_kWh / Home_Area_Sqft Usage_Status "High Usage" if Monthly_Consumption_kWh is above 350, otherwise "Normal" AC_Usage_Band AC_Hours_Per_Day grouped into Low (0-3), Medium (3-7) and Heavy (7+)
Findings

Question 1: What does the consumption look like overall? 
Statistic Value (kWh) Households (count) 250 Mean 245.8 Standard deviation 82.4 Minimum 58.0 25th percentile 190.2 Median 240.0 75th percentile 300.2 Maximum 529.0
The mean (245.8 kWh) and the median (240.0 kWh) are close, and the skewness is only 0.28, so the distribution is nearly symmetric with a slight tail towards high values. A typical household uses about 240 kWh per month, and most households fall between 190 and 300 kWh.

Distribution of monthly consumption

Question 2: Do AC hours and home area affect consumption?

Yes, both do. Correlation of each numeric column with Monthly_Consumption_kWh:

Column Correlation AC_Hours_Per_Day 0.59 Home_Area_Sqft 0.50 Avg_Temperature_C 0.37 Household_Size -0.01 Appliance_Age_Years -0.09

Average consumption by AC usage band:

AC usage band Households Average kWh Median kWh Low (0-3) 70 187.1 179.5 Medium (3-7) 120 248.3 243.0 Heavy (7+) 60 309.2 300.0

Households with heavy AC use consume about 122 kWh more per month than households with low AC use. Larger homes also use more electricity. Temperature has a moderate link with consumption; hotter places also tend to run the AC for longer, so the two effects go together. Household_Size and Appliance_Age_Years show almost no relationship with consumption in this data.

Scatter plots of AC hours and home area

Correlation heatmap

Average consumption by AC usage band

Question 3: Does consumption differ by group?

Average consumption by group (sorted from highest to lowest):

City

City Households Average kWh Median kWh Mumbai 54 260.8 264.0 Kolkata 25 259.1 243.0 Delhi 66 252.6 247.0 Chennai 46 244.4 239.0 Bengaluru 59 219.9 210.0

Property type

Property type Households Average kWh Median kWh Independent House 60 271.7 283.0 Builder Floor 57 245.7 240.0 Apartment 133 234.2 229.0

Tariff plan

Tariff plan Households Average kWh Median kWh Standard 150 250.3 239.5 Time of Day 68 240.0 237.5 Prepaid Smart 32 237.2 248.0

Rooftop solar

Has solar Households Average kWh Median kWh No 215 251.1 244.0 Yes 35 213.2 217.0

Bengaluru households use the least electricity (219.9 kWh on average), while Mumbai is the highest (260.8 kWh). Independent Houses use more than Apartments (271.7 against 234.2 kWh), which fits with their larger size. Households with rooftop solar use about 38 kWh less than those without. The three tariff plans are close to each other (237 to 250 kWh), so the tariff plan shows little effect in this data.

Average consumption by category

Question 4: Which households need attention?

A household is High Usage when it uses more than 350 kWh in a month.

Usage status Households Share Normal 224 89.6% High Usage 26 10.4%

Average of each column, High Usage compared with Normal:

Column High Usage Normal Home_Area_Sqft 1693.7 1102.1 AC_Hours_Per_Day 8.1 4.6 Household_Size 3.3 3.4 Avg_Temperature_C 30.4 26.6 Appliance_Age_Years 5.2 6.4 Monthly_Consumption_kWh 394.8 228.5

Share of High Usage households by group:

Group High Usage share City: Kolkata 20.0% (5 of 25) City: Mumbai 16.7% (9 of 54) City: Chennai 10.9% (5 of 46) City: Bengaluru 6.8% (4 of 59) City: Delhi 4.5% (3 of 66) Property type: Independent House 18.3% Property type: Builder Floor 8.8% Property type: Apartment 7.5% Solar: No 11.6% Solar: Yes 2.9%

The 26 High Usage households have much larger homes (about 1694 sqft against 1102 sqft), run the AC longer (8.1 hours a day against 4.6) and live in hotter places. Household size is almost the same in both groups. Kolkata and Mumbai have the highest share of High Usage households, and Independent Houses are far more likely to be High Usage than Apartments.

High Usage and Normal households

High Usage share by city

Recommendations Target the High Usage households (large homes with heavy AC use) with energy-saving advice, such as AC maintenance, better settings and efficient appliances. Promote rooftop solar, especially for Independent Houses and for households in Kolkata and Mumbai, because households with solar use less electricity from the grid. Plan supply for hot periods and larger homes, since AC hours and home area are the two strongest drivers of consumption. Review the tariff design: the tariff plans show little difference in consumption here, so they are unlikely to be the main way to reduce high usage.
Limitations The dataset is synthetic, so the results show patterns in this data only and not in real households. The dataset is small (250 households); some groups are small (for example, 25 households in Kolkata), so group differences should be read with care. A correlation shows that two columns move together; it does not prove that one causes the other.
Project Structure household-electricity-consumption/
├── data/ │
      ├── raw/ electricity_data.csv │
      └── processed/ electricity_data_clean.csv
├── notebooks/ cleaning, new columns and analysis code 
├── images/ charts (PNG files)
├── documents/ SRS document
└── README.md
How to Run
Run these from the notebooks folder, in this order:

clean_data.py: cleans the raw data and saves it to data/processed create_new_columns.py: adds Usage_Per_Sqft, Usage_Status and AC_Usage_Band analysis_and_charts.py: prints the tables and saves the charts to images

Requirements: Python 3, pandas, NumPy, Matplotlib and Seaborn.
