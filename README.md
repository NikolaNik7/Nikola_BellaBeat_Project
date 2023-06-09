# Nikola_BellaBeat_Project

Author: Nikola Nikolic 

Date: 4/06/2023

This Project will follow the six step data analysis process: 
### ❓ [Ask](#1-ask)
### 💻 [Prepare](#2-prepare)
### 🛠 [Process](#3-process)
### 📊 [Analyze](#4-analyze)
### 📋 [Share](#5-share)
### 🧗‍♀️ [Act](#6-act)

## Scenario
According to a Forbes report, Bellabeat, a wellness tech company, was established in 2013 by Sandro Mur, Urška Sršen, and Lovepreet Singh (Robter, 2020). While its headquarters are in San Francisco, the company has a global presence with offices in London, Hong Kong, and Zagreb. Bellabeat specializes in women's health and wellness, offering a range of wearable and non-wearable technologies. Their brand is built upon four core principles: smart insights, women-centric approach, holistic well-being, and body positivity. Their products aim to incorporate various metrics related to a woman's menstrual cycle. The company's website states that Bellabeat helps users synchronize with their natural cycle and gain a deeper understanding of their well-being by tracking bio-responses and aligning the data with their hormonal cycle, enabling them to comprehend their emotional and physical states better.

## 1. Ask
1.1  Business Task
Uilizing the Fitbit Fitness Tracker Data, identify some trends in smart device usage, how these trends can be applied to Bellabeat’s customers and how they can help influence Bellabeat’s marketing strategy.

1.2 Stakeholders
Urška Sršen – Bellabeat’s cofounder and Chief Creative Officer
Sando Mur – Bellabeat’s cofounder
Bellabeat’s marketing analytics team

## 2. Prepare 
2.1 Data Used
The data source used for this case study is FitBit Fitness Tracker Data. This dataset was downloaded from Kaggle where it was uploaded by Möbius.

2.2 Accessability and Usage of Data 
The dataset was published by Möbius to Kaggle.com under the CC0: Public Domain Creative Common License – waiving all rights to the work and allowing for the dataset to be copies, modified, distributed and performed without asking for permission: https://zenodo.org/record/53894#.YMoUpnVKiP9 

2.3 Data Summary 
According to the dataset information submitted on Zenodo.org,"this dataset was generated by respondents to a distributed survey via Amazon Mechanical Turk between 03.12.2016 – 05.12.2016.Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring." Additionally, "Variation between output represents use of different types of Fitbit trackers and individual tracking behaviors". 

2.4 Data Organisation 
Eighteen datasets were downloaded from the FitBit Fitness Tracker Data. The datasets downloaded in .csv file format and included long and wide formats. The datasets chosen for analysis below included a user count of 33 participants over a 31 day period of time.

2.5 Data Limitations and Integrity 
The FitBit Fitness Tracker Data was collected in 2016 making the datasets outdated for current trend analysis. Additionally, while the data initially states a time range of 03-12-2016 to 05-12-2016, after data verification, the data collected was only during a 31 day period (04-12-2016 to 05-12-2016). Since the data only included instances over a 31 day period, the timeframe for a more insightful analysis is realitively small.

Lastly, the sample size itself could create a sample bias. While a sample size of 33 will hold up within the CLT theorm, a larger sample size will be more representative of the population and would increase the confidence interval. Additionally, since there were no demographic information collected it will be hard to see if we have a true representation of a national or global population. This lack of demographic information will also limit recommendations on the target audience (including gender, location, age and job status) and where to market to them. Considering that Bellabeat is primarily targeted to women and individuals who menstruate, having demographics would have bolstered any recommendations after analysis.

## 3. Process
3.1 Datasets Selected 
  - Daily_Activity_Merged
  - Daily_Sleep_Merged
  - Hourly_Steps_Merged
  - Hourly_Intensity_Merged
  - Hourly_Calories_Merged
  - Weight_Log_Merged

3.2 Cleaning Data Via Google Sheets
 Each dataset was cleaned using Google Sheets. The following steps were taken within each dataset:
  - Sorted and filtered data by Id to obtain how many unique users there were within the dataset.
  - Checked for duplicate data using the 'Remove Duplicates' tool in Google sheets 
  - Formatted date data into MM/DD/YY date format
  - Formatted all numerical data into Number format with either no decimals or up to 2 decimials.
  - Sorted by date to find the first and last date of the dataset (this is what first indicated only a 31-day period of activity was captured).
  - Separated Date and Hour into two columns when needed for later analysis. Utilized Left() function as well as Right() function to split.
  - Formatted any time data into 00:00:00 format for consistency.
  - Checked Id entries and other columns for LEN to make sure the data was correct and uniform in length
  - Used Conditional Formatting to spot any errors that didn't equal to what I had specified 
  - Used the 'Hide' setting to remove unnecessary columns that contained irrelevant information 

After the cleaning process was finished, only 3 rows of duplicate information was found within the Daily_Sleep_Merged file. These were removed before analysis.

## 4. Analyze

4.1 SQL Dataset Upload 
Uploaded the following clean data sets:

  - dailyActivity_merged
  - sleepDay_merged
  - hourlyCalroie_merged
  - hourlyIntensity_merged
  - hourlySteps_merged
  - weightLogInfo_merged

4.2 User Verification
Checked for the number of participants by counting the number of distinct Id's in each dataset.
```
SELECT COUNT(DISTINCT Id) AS Total_Id
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
```

I repeated the SQL query above with each dataset (changing the FROM clause each time) and recieved these results:

Daily_Activity_Merged - 33
Daily_Sleep_Merged - 24
Hourly_Calorie_Merged - 33
Hourly_Intensity_Merged - 33
Hourly_Steps_Merged - 33
Weight_Log_Merged - 8

The Weight Log dataset did not include enough data to move forward with analysis. The dataset will not be used.

4.3 User Insights 
Now I wanted to  breakdown the users by how much they wore their FitBit Fitness Tracker. I created three user types:

Active User - wore their tracker for 21-31 days
Moderate User - wore their tracker for 11-20 days
Light User - wore their tracker for 1 to 10 days

```
SELECT Id,
COUNT(Id) AS Total_Logged_Uses,
CASE
WHEN COUNT(Id) BETWEEN 21 AND 31 THEN 'Active User'
WHEN COUNT(Id) BETWEEN 11 and 20 THEN 'Moderate User'
WHEN COUNT(Id) BETWEEN 1 and 10 THEN 'Light User'
WHEN COUNT(Id) = 0 THEN 'Inactive User'
END Fitbit_Usage_Type
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
GROUP BY Id
```

https://docs.google.com/spreadsheets/d/17FSZmmafb-hJfILoUGUe_IF7MJOvBrERI8lZaDt-luo/edit?usp=sharing
https://public.tableau.com/app/profile/nikola.nikolic/viz/UsageDataBellabeat/Sheet1

Next, I wanted to take a closer look at the MIN, MAX, & AVG of total steps, total distance, calories and activity levels by Id.

```
SELECT Id,
MIN(TotalSteps) AS Min_Total_Steps,
MAX(TotalSteps) AS Max_Total_Steps, 
AVG(TotalSteps) AS Avg_Total_Stpes,
MIN(TotalDistance) AS Min_Total_Distance, 
MAX(TotalDistance) AS Max_Total_Distance, 
AVG(TotalDistance) AS Avg_Total_Distance,
MIN(Calories) AS Min_Total_Calories,
MAX(Calories) AS Max_Total_Calories,
AVG(Calories) AS Avg_Total_Calories,
MIN(VeryActiveMinutes) AS Min_Very_Active_Minutes,
MAX(VeryActiveMinutes) AS Max_Very_Active_Minutes,
AVG(VeryActiveMinutes) AS Avg_Very_Active_Minutes,
MIN(FairlyActiveMinutes) AS Min_Fairly_Active_Minutes,
MAX(FairlyActiveMinutes) AS Max_Fairly_Active_Minutes,
AVG(FairlyActiveMinutes) AS Avg_Fairly_Active_Minutes,
MIN(LightlyActiveMinutes) AS Min_Lightly_Active_Minutes,
MAX(LightlyActiveMinutes) AS Max_Lightly_Active_Minutes,
AVG(LightlyActiveMinutes) AS Avg_Lightly_Active_Minutes,
MIN(SedentaryMinutes) AS Min_Sedentary_Minutes,
MAX(SedentaryMinutes) AS Max_Sedentary_Minutes,
AVG(SedentaryMinutes) AS Avg_Sedentary_Minutes
From `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
Group BY Id
```
Google Sheets Results link here 

Then I wanted to narrow my results to just the averages of the different types of minutes by Id.

```
SELECT Id, 
avg(VeryActiveMinutes) AS Avg_Very_Active_Minutes,
avg(FairlyActiveMinutes) AS Avg_Fairly_Active_Minutes,
avg(LightlyActiveMinutes) AS Avg_Lightly_Active_Minutes,
avg(SedentaryMinutes) AS Avg_Sedentary_Minutes,
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
GROUP BY Id
```
Google Sheets Results link here 

Results showed that the average minutes of the Sedentary activity level was the highest for each distinct Id.

Lastly, I wanted to take a look at average active minutes by week day before moving on to user types. In Google sheets I used the function "=CHOOSE(WEEKDAY(L2), "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday")", in order to change the dates to days of the week.

```
SELECT Weekday,
ROUND(avg(VeryActiveMinutes), 2) AS Avg_Very_Active_Minutes,
ROUND(avg(FairlyActiveMinutes), 2) AS Avg_Fairly_Active_Minutes,
ROUND(avg(LightlyActiveMinutes), 2) AS Avg_Lightly_Active_Minutes,
ROUND(avg(SedentaryMinutes), 2) AS Avg_Sedentary_Minutes,
FROM `eighth-breaker-387002.CS2_bellabeat.DailyActivityMergedWeekday`
GROUP BY Weekday;
```
https://public.tableau.com/app/profile/nikola.nikolic/viz/AVG_MIN_WEEKDAY_INTENSITY/Sheet1#1

Through this query we see that Sedentary Minutes are the highest type of active minutes. What is noticable is that there is no real difference in type of active minute total by week day. It seems users are consistent in their active minute output each day. This information could show that Bellabeat could leverage activity goals for users to meet as users might already being trying to meet personal activity goals each day and Bellabeat could enourage higher activity goals to increase daily active minutes that are very active or fairly active.

4.4 User Types by Activity Levels 
4.4.1
According to the Australian Government, Department of Health and Aged Care, the physical activity and exercise guidelines for Australians aged 18-64 years old is to be active on most (preferably all) days, to weekly total minimum of: 2.5 hours of moderate activity or 1.25 hours of vigorous activity or an equivalent combination of both.   

So minimum is 2.5h + 1.25h = 3.75h = 225 minutes

Notation of SQL will use SUM() function to add together all 31 days then divide by 4 to obtain the 1 week total. 

```
SELECT Id, 
SUM(VeryActiveMinutes)/4 + SUM(FairlyActiveMinutes)/4 AS Total_SUM_Active_Minutes,
CASE 
WHEN SUM(VeryActiveMinutes + FairlyActiveMinutes) >= 225 THEN 'Meets Recommended Australian Guidelines'
WHEN SUM(VeryActiveMinutes + FairlyActiveMinutes) <225 THEN 'Does Not Meet Recommended Australian Guidelines'
END Aus_Activity_Guidelines
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
GROUP BY Id
```
Google Sheets Results link here 
[Link to Tableu] and/or post excel chart here

Of the 33 participants, 25 had met the Australian Guidelines, whereas the remaining 8 did not. 

4.4.2
A Healthline.com article (“How many steps do I need a day?”) written by Sara Lindberg in 2019 cited a 2011 study by Tudor-Locke et. al. titled “How many steps/day are enough? for adults” which found that 10,000 steps/day is a reasonable target for healthy adults. Lindberg (2019) breaks down activity level by steps into three categories based off the 2011 study by Tudor-Locke et. al.:

Inactive: less than 4,000 steps per day
Average (somewhat active): ranges from 7,500 to 9,999 steps per day
Very Active: more than 13,500 steps per day

I will be using the above activity categories to create user types for the distinct Ids within the Daily Activity dataset. I’m interested to see how these categories may be broken down. When creating my SQL query I realized that the above categories from the Healthline article left some gaps. So I created two other categories:

Low Active User: 4,000 to 7,499 steps
Active User: 10,000 to 13,499 steps

```
SELECT Id,
round(avg(TotalSteps),2) AS Avg_Total_Steps,
CASE
WHEN avg(TotalSteps) < 4000 THEN 'Inactive'
WHEN avg(TotalSteps) BETWEEN 4000 AND 7499 THEN 'Low Active User'
WHEN avg(TotalSteps) BETWEEN 7500 AND 9999 THEN 'Average Active User'
WHEN avg(TotalSteps) BETWEEN 10000 AND 13499 THEN 'Active User'
WHEN avg(TotalSteps) >= 13500 THEN 'Very Active User'
END User_Type
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
GROUP BY Id
```
Google Sheets Results link here 
[Link to Tableu] and/or post excel chart here

Here are the Results:

Inactive User: 8 users
Low Active User: 9 users
Average Active User: 9 users
Active User: 5 users
Very Active User: 2 users

If we break this down between Non & Low Active users and users who are considered ‘Active’ we can see that 17 users (52%) are little to non-active and 16 (48%) users are considered active. So almost a 50/50 split on type of users.

4.5 Calories, Steps & Active Minutes by ID
Now that we have more insights into our users’ activity levels, I’m interested in seeing what their logged calories in relation to their steps and active minutes can tell us.
```
SELECT Id, 
Sum(TotalSteps) AS Sum_total_steps,
SUM(Calories) AS Sum_Calories, 
SUM(VeryActiveMinutes + FairlyActiveMinutes) AS Sum_Active_Minutes
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged`
GROUP BY Id
```
Google Sheets Results link here 
[Link to Tableu] and/or post excel chart here

4.6 Total Steps by Day
Next, I wanted to take a look at average steps by day to see if users were more active on certain days of the week.

```
SELECT Weekday,
ROUND (avg(TotalSteps), 2) AS Average_Total_Steps,
FROM `eighth-breaker-387002.CS2_bellabeat.DailyActivityMergedWeekday`
GROUP BY Weekday
ORDER BY Average_Total_Steps DESC 
```
Google Sheets Results link here 
[Link to Tableu] and/or post excel chart here

After running the query, there wasn't a whole lot of difference between each day in terms of average steps. With that said, Saturday had the highest average steps as well as the beginning of each week (Monday and Tuesday). We could potentially infer from this that the users wanted to be more active right after the weekend of rest (Sunday with lowest total steps & Friday not too far behind) & that Saturday allowed for more time for activity & movement.

4.7 Deeper look into Sleep
Then I wanted to explore the Sleep habits of the users and how it compares to activity level.
```
SELECT a.Id,
round(avg(a.TotalSteps),2) AS AvgTotalSteps,
round(avg(a.Calories),2) AS AvgCalories,
round(avg(s.TotalMinutesAsleep),2) AS AvgTotalMinutesAsleep,
FROM `eighth-breaker-387002.CS2_bellabeat.Daily_Activity_Merged` AS a
INNER JOIN `eighth-breaker-387002.CS2_bellabeat.Sleep_Day_merged` AS s ON a.Id=s.Id
GROUP BY a.Id 
```
Google Sheets Results link here 
[Link to Tableu] and/or post excel chart here

Then I visualized average total steps against average total minutes slept to see any type of correlation

[Link to Tableu] and/or post excel chart here

## 5. Share 
My Complete dashboard on Tableu will be put here in due time.

## 6. Act
Bellabeat's women-centric, holistic approach paired with smart insights and body positivity has led to the creation of wearable technology for women. These products empower women to utilize data to improve their overall health.

Since Bellabeat focuses strongly on a female audience for their products, I would recommend that the company look into using their own marketing and user data or conduct their own data collection to gain further insights and trends. I'd also recommend using a larger sample size if possible in order to increae the confidence interval. Since the data utilized in this case study did not include demographic information, I'm unable to give a more detailed recommendaion or ensure there was no sampling bias.

App Notifications
  - Through my analysis I found about 7% percent of users were 'low' or 'moderate' when it came to wearing their FitBits. While most of the users wore thier           FitBits consistently, I'd recommend a daily notification at the end of day to 'charge up' the wearable so it's ready for tracking the next day.

  - The analysis also showed that 52% of users are not reaching the recommended 10,000 steps a day. Adding in notifications throughout the day as reminders to         "get up & move" or to "complete your step goal for the day" may help increase total steps for users.
 
  - Lastly, the data showed that users who averaged 5 hours of sleep or more also had a higher average step total. It may be beneficial to help users increase        their sleep time by sending them a notification to 'wind down for sleep' at a certain time based off their sleep habits.

Better Data Pipeline/Gathering
  - Bella Beat should consider making it mandatory that all customers input certain information about themselves to better allow data analysts to manipulate data   more meaningfully. One example, is to make people input their height and weight before allowing access to their applications. This would provide Bella Beat       analyst greater access into the insights of their customers and will allow them to make more informed suggestions to their clients, as in the example given,       including height and weight would allow us to find other indicators such as BMI (which is Kg/M). This greater increase in information would allow bella beat       to  make better decisions moving forward in regards to marketing and product designing. 
















