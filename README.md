# Urban-Heatwave-Dashboard
Power BI project analyzing urban heatwave trends across 3 Indian metrocities (2002–2022)

### Dashboard Link : 

## Problem Statement

Rising temperatures and urbanization have amplified the intensity and frequency of heatwaves across Indian metropolitan cities. This project aims to analyze heatwave patterns, identify vulnerable cities and time periods, and quantify population exposure across Delhi, Kolkata, and Bengaluru between 2002 and 2022.



### Steps followed 

#### 1. Python (Jupyter Notebook) for Data Preparation
- Step 1 : Loaded csv file in Jupyter and cleaned it checking for null, duplicate and NA values.
- Step 2 : Added "Date" coloumn to the csv file using pandas. Since, "Day of the Year" column isnt useful as such, therefore, removed "Day of Year" column. Swapped "Date" and "Year" column to be in prper order.
- Step 3 : Created column to tag heatwave days based on "T2m_max" (Temperature) thresholds.
- Step 4 : Added "City" tag by creating "City" coloumn. Saved this file as "clleaned_cityname.csv".
- Step 5 : Going by the Census data, for 2002-2022, only two datapoints were available, i.e., population data from 2001 Census and population data from 2011 Census. So, the next steps included interpolation and extrapoliation of the population,
  
  (a) First, Interpolation of  population values for 2001–2011 was performed using linear interpolation.
  
  (b) Next, Extrapolated population for 2012–2022 using calculated growth rate.
- Step 6 : Next, saved this population file as "population_cityname.csv".
- Step 7 : By now, both cleaned daily temperature dataset and population dataset are ready. Finally, merged daily temperature dataset with population data. Saved this file as "merged_cityname.csv".
- Step 8 : Repeated all the above steps for each city. Therefore, 3 csv files of merged datasets.

####   2. Power BI Dashboard Building
- Step 9 : Imported all 3 merged files. Appended Delhi, Kolkata and Bengaluru tables to one table "Combined".
- Step 10 : Created calculated columns & measures using DAX.

  (a) Columns:

  - Index = RANKX(ALL('Combined'),'Combined'[DATE],'Combined'[DATE], ASC)
  - is_hw = IF(Combined[HEATWAVE] = TRUE(), 1,0)
  - MonthName = FORMAT('Combined'[DATE], "MMMM")
  - HeatwaveGrp = 
   VAR CurrentIndex = 'Combined'[Index]
   VAR PreHeatwave = CALCULATE(MAX('Combined'[Index]), FILTER('Combined', 'Combined'[HEATWAVE]    = FALSE() && 'Combined'[Index] < CurrentIndex))
   RETURN CurrentIndex - PreHeatwave

   (b) Measures:
  - HeatwaveperMonth = CALCULATE(COUNTROWS('Combined'), 'Combined'[HEATWAVE] = TRUE())
  - Heatwaveperyear = CALCULATE(COUNTROWS(Combined), Combined[HEATWAVE] = TRUE())
  - hw_days = SUM(Combined[is_hw])
  - hw_per_million = DIVIDE(SUM(Combined[is_hw]), SUM(Combined[POPULATION]) / 1000000)
  - LongestHWStreak = MAXX(FILTER('Combined', 'Combined'[HEATWAVE] = TRUE()), 'Combined'[HeatwaveGrp])
  - percentHeatwave = DIVIDE(CALCULATE(DISTINCTCOUNT(Combined[DATE]), Combined[HEATWAVE] = TRUE()), DISTINCTCOUNT(Combined[DATE])) * 100

           
        
- Step 10 : Built KPIs, maps, line and bar charts, tables, and text insights. Customized layout, color theme, tooltips, and interactions.
- Step 11 : Visual filters (Slicers) were added for four fields named "City" and "Year".

# Snapshot of Dashboard (Power BI Service)

![dashboard_snapo]
 
 # Report Snapshot (Power BI DESKTOP)

 
![Dashboard_upload]


# Insights

### 1. Heatwave Days

- “More than 1 in 5 days (21.42%) saw a heatwave in at least one metro."

- “Delhi peaked at 48.64°C, topping all 10 hottest days."

- “Delhi and Kolkata show increasing heat stress, especially post-2010."

### 2. Bengaluru’s Resilience

- “Bengaluru records near-zero HW days, showing climate resilience."

### 3. Delhi Dominates Extremes

- “All Top 10 hottest days occurred in Delhi (2003–2019)."

- “Early June emerges as the most dangerous heatwave window."

### 4. Population Exposure

- “Kolkata sees moderate HW count, but high HW per million due to dense population."

- “Delhi faces 4x higher per capita HW burden than Bengaluru."

### 5. Bonus Text Cards Used Inside Dashboard:

- “1 in 5 days is a heatwave across metros."

- “Delhi peaked at 48.64°C, & dominates top 10 hottest days."

- “Bengaluru remains the most heatwave-resilient."


