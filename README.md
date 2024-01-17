# City Bike Trips in Chicago, Illinois, February 2023

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)

### Project Overview

This is a case study of a bike-share program in Chicago, Illinois - the datasets were provided by the Google Data Analytics course. The data has been made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement).
This case study will be focusing on the "Cyclistic" bike-share program.

In this case study, we will be focusing on this bike-share data for February 2023.  There are three main questions we will consider:
  1. How do annual members and casual riders use Cyclistic bikes differently?
  2. Why would casual riders buy Cyclistic annual memberships?
  3. How can Cyclistic use digital media to influence casual riders to become members?

### Data Sources

[2023 Chicago City Bike Data](https://divvy-tripdata.s3.amazonaws.com/index.html) - Dataset provided by Motivate International Inc.
  - 202302-divvy-tripdata.csv - raw data file
  - TripData1.csv - cleaned data file

### Tools

  - Google Sheets - Data Cleaning
  - SQL Server - Data Analytics
  - Tableau - Data Visualization

### Data Cleaning and Preparation

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
--In the first query for analysis, we'll look into total users by day of the week


--Pulling the total number of users by day of the week

SELECT count(member_casual) as users, day_of_week
FROM `bikes-407115.TripData.February`
GROUP BY day_of_week
ORDER BY users asc
-- Tuesday is the most popular day of the week for riders
-- Friday is the least popular day for riders, with Thursday trailing very close behind it
-- There is a steady, upward trend of users from Friday to Tuesday - the dip in users is from Wednesday to Friday

```

### ![TotalUsersDayWeek](https://github.com/tinagrillo/ChicagoBikeTrips/assets/31528924/632412d8-2693-43c3-af3a-f4bf47850cc8)

```sql
--For the next set of queries, we dive into the differences between casuals and members:


--Pulling the percentage of users who are members vs casuals

SELECT
count(member_casual) as totalusers,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "casual") as casualtotal,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "member") as membertotal,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "casual")/count(member_casual)*100 as casualpercent,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "member")/count(member_casual)*100 as memberpercent
FROM `bikes-407115.TripData.February`
-- 77% of users are members while 23% of users are casual


--Total Users (percentage) grouped by Day of Week

SELECT day_of_week, count(member_casual) as users, count(member_casual)/(select count(member_casual)from `bikes-407115.TripData.February`)*100 as percent
FROM `bikes-407115.TripData.February`
GROUP BY day_of_week
ORDER BY percent desc
day_of_week,users,percent
-- ~19% of users ride on Tuesdays
-- ~16% of users ride on Mondays
-- ~16% of users ride on Sundays
-- ~14% of users ride on Wednesdays
-- ~13% of users ride on Saturdays
-- ~11% of users ride on Thursdays
-- ~11% of users ride on Fridays
--Exported as PercentTotal_DayOfWeek.csv (attached file)


--Percent of Users by Day of Week and Member vs Casual

SELECT day_of_week, member_casual,
count(member_casual) as users,
count(member_casual)/(select count(member_casual)from `bikes-407115.TripData.February`)*100 as percent
FROM `bikes-407115.TripData.February`
GROUP BY day_of_week, member_casual
ORDER BY percent desc
-- the highest percent of users being 15% for members on Tuesdays
-- the lowest percent of users being 2% for casual users on Thursdays
-- the highest percent of casual users is 5% on Sundays
-- the lowest percent of members is 9% on Fridays and Saturdays
--From this, we've established that members are substantially more active on Tuesdays coming out of the weekend while casual users are the most active on weekends, with Sundays being the most active.
--Exported as PercentUsers_DayOfWeek.csv (attached file)
```

### ![PercentageUsersDayWeek](https://github.com/tinagrillo/ChicagoBikeTrips/assets/31528924/24bb0530-6435-4814-83f8-fb9ffbaf36dc)

```sql


-- The next set of queries will dive into users who use CLASSIC bikes, ELECTRIC bikes, and DOCKED bikes:


--testing out simple query to display total users grouped by member/casual and bike type:

SELECT count(member_casual) as users, rideable_type, member_casual
FROM `bikes-407115.TripData.February`
GROUP BY member_casual, rideable_type
ORDER BY users desc
-- Members using classic bikes slightly outnumber members using electric bikes


--Percentages and totals of users using each type of bike:

SELECT count(member_casual) as totalusers, 
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "casual") as casualtotal, 
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "member") as membertotal, 
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "casual" AND rideable_type = "classic_bike")/count(member_casual)*100 as casualclassicpercent,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "member" AND rideable_type = "classic_bike")/count(member_casual)*100 as memberclassicicpercent,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "casual" AND rideable_type = "electric_bike")/count(member_casual)*100 as casualelectricpercent,
(select count(member_casual) from `bikes-407115.TripData.February` where member_casual = "member" AND rideable_type = "electric_bike")/count(member_casual)*100 as memberelectricpercent,
(select count(member_casual) from `bikes-407115.TripData.February` where rideable_type = "docked_bike")/count(member_casual)*100 as dockedbikepercent
FROM `bikes-407115.TripData.February`
-- ~39% of users are members using classic bikes
-- ~38% of users are members using electric bikes
-- ~13% of users are casuals using electric bikes
-- ~8% of users are casuals using classic bikes
-- ~1% of users casuals using docked bikes (0% are members)
--Exported as PercentUsers_RideableType.csv (attached file)

--Testing/verifying totals between users (member vs casual), grouped by day of the week

SELECT day_of_week,
count(member_casual) as users,
(select count(*) from `bikes-407115.TripData.February` where member_casual = "member") as members,
(select count(*) from `bikes-407115.TripData.February` where member_casual = "casual") as casuals
FROM `bikes-407115.TripData.February`
GROUP BY day_of_week
ORDER BY users desc

--Percentages of users grouped by Day of Week and Member/Casual:

SELECT day_of_week, member_casual,
count(member_casual) as users,
count(member_casual)/(select count(member_casual)from `bikes-407115.TripData.February`)*100 as percent
FROM `bikes-407115.TripData.February`
GROUP BY day_of_week, member_casual
ORDER BY percent desc


SELECT rideable_type, day_of_week, count(member_casual) as users, member_casual, count(member_casual)/(select count(member_casual)from `bikes-407115.TripData.February`)*100 as percent
FROM `bikes-407115.TripData.February`
GROUP BY rideable_type, day_of_week, member_casual
ORDER BY percent desc
--Final table used to easily show distribution of users and bike types throughout the week
--Exported as ExpandedRideableType_PercentUsers.csv (attached file)
```
![TotalUsersBikeType](https://github.com/tinagrillo/ChicagoBikeTrips/assets/31528924/e38491b1-d6e1-4c69-9a0f-ca241dad82d0)

After importing the data table into Tableau, this vizualization easily shows the number of users by different bike types and day of week.
Here we can easily see how the members greatly exceed casual users
- Classic bikes are slightly more preferred amongst members
- Electric bikes are much more preferred amongst casual users
- Only casual users show using a 'docked' bike

Although "docked" bike data shows in this visualization, it has been largely treated as outlier data for analysis purposes;

In this last visualization, we will omit the docked bike data

### Other Visualizations

Created in Tableau, the table below focuses on users' preferred time of day to ride:
![TimeDayBikeType](https://github.com/tinagrillo/ChicagoBikeTrips/assets/31528924/0d53ebe8-2e8d-444d-b062-d341d5b8a98e)
We can see here that across all types of bikes, 4pm-5pm is the preferred time to ride
- Amongst members, 5pm is the obvious preferred time
- Amongst casual users, 4pm is slightly more preferred to 5pm
- Trend lines in graph show that the overall trend between members is nearly identical
  - For casuals using electric bikes, there is a higher preference for morning times around 7am-8am than with classic bikes

### Results
[Link to Tableau Dashboard](https://public.tableau.com/app/profile/bettina.grillo/viz/February2023ChicagoCityBikeTrips-Project2/DashboardDark)
  1. Tuesday is the most popular day to ride a bike overall - Sunday is the most popular day for casual users
  2. There are much more members riding than casual users - 23% casual users vs 77% members
  3. Electric bikes were the most commonly used - 52% of all users
     - 47% of users used classic bikes
     - 1% of users used docked bikes (casual users only)
  4. The most popular time to ride across all users is between 4pm-5pm
  5. Members have almost no preference for classic or electric bikes while casual users seem to prefer electric bikes
    
### Recommendations
- Focus on the selling the strengths of members
- Digital Media Marketing - Hire influencers to vlog/showcase the benefits and lifestyle of being a member in the Cyclist program
- Maintain and foster the relationship with members - maintain or provide new incentives (i.e. products, services, discounts, sales)
- Initiate incentive/marketting strategy that overlaps with the most popular times that users ride
