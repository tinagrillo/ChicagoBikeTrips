# City Bike Trips in Chicago, Illinois, February 2023

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaning-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#results-findings)
- [Recommendations](#recommendations)

### Project Overview

Description of Project

### Data Sources

[2023 Chicago City Bike Data](https://divvy-tripdata.s3.amazonaws.com/index.html) - Dataset provided by the Google Analytics Certificate Program
  - .csv
  - .csv

### Tools

  - Google Sheets - Data Cleaning
  - SQL Server - Data Analytics
  - Tableau - Data Visualization

### Data Cleaning/Preparation

To clean the data, the following steps were taken:
1. Uploaded .csv files into Google Sheets
2. Removed duplicate entries
3. Data formatting
4. Prepared data for analysis by adding columns such as:
   - ride_length (subtracted column "ended_at" from "started_at")
   - day_of_week (google sheets =weekday function applied to "started_at" column)
   
### Exploratory Data Analysis

Leading questions for analysis:
  - How do annual members and casual riders use Cyclistic bikes differently?
  - Why would casual riders buy Cyclistic annual memberships?
  - How can Cyclistic use digital media to influence casual riders to become members?

### Data Analysis

```sql
SELECT count(*) as Users, member_casual
FROM `bikes-407115.TripData.February`
GROUP BY member_casual;
```

### Results/Findings
  1. Tuesday is the most popular day to ride a bike overall - Sunday is the most popular day for casual users
  2. There are much more members riding than casual users - 23% casual users vs 77% members
  3. Electric bikes were the most commonly used - 52% of all users
     - 47% of users used classic bikes
     - 1% of users used docked bikes (casual users only)
    
### Recommendations
- Focus on the selling the strengths of members
- Digital Media Marketing - Hire influencers to vlog/showcase the benefits and lifestyle of being a member in the Cyclist program
- Maintain and foster the relationship with members - maintain or provide new incentives (i.e. products, services, discounts)
