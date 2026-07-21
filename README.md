BrightTV Viewership Analysis

1. Project Overview

This project analyzes BrightTV viewership data to understand customer viewing behaviour, identify trends in content consumption, and provide data-driven recommendations to help increase user engagement and grow the subscriber base.
The analysis was completed as part of a business case study where the objective was to support management with actionable insights that can improve customer retention and increase platform usage.

2. Business Objectives

Understand how customers consume content.
Identify trends in viewing behaviour.
Discover factors that influence content consumption.
Recommend strategies to increase viewership during low-consumption periods.
Support business growth through data-driven decision-making.

3. Dataset

The project uses two datasets:

User Profile Dataset – Contains customer demographic information such as age, gender, province, and subscription details.
Viewership Dataset – Contains viewing activity including watch dates, watch times, channels, programmes, and viewing duration.

4. Tools Used

Databricks
SQL
Microsoft Excel
PowerPoint
Data Studio
PowerBI
Miro
Canva

5. Throughout this project, the following SQL techniques were used:

Common Table Expressions (CTEs)
CASE Statements
Aggregate Functions
GROUP BY
ORDER BY
DISTINCT
Date Functions
Data Cleaning
Data Transformation
Filtering
Data Categorisation

6. Analysis Performed

The analysis focused on answering key business questions, including:

Which days have the highest and lowest viewership?
Which hours are the busiest?
Which TV channels receive the most views?
How does viewing behaviour differ between weekdays and weekends?
Which customer age groups are the most active?
Which provinces have the highest number of viewers?
Monthly and daily viewing trends.
Overall customer viewing patterns.
Key Insights

7. Some of the insights identified include:
8. 
Peak viewing hours occur during specific times of the day.
Weekend viewing patterns differ from weekday behaviour.
Certain TV channels consistently attract higher audiences.
Customer demographics influence viewing habits.
Seasonal and monthly trends impact overall engagement.


MY CTE EXAMPLE 

WITH User_profile AS (
    SELECT userid,
        CASE 
            WHEN Province = ' ' OR Province = 'None' THEN 'Uncategorized'
            ELSE Province
        END AS Region,

        CASE 
            WHEN Age = 0 THEN 'Infants'
            WHEN Age BETWEEN 1 AND 12 THEN 'Children'
            WHEN Age BETWEEN 13 AND 19 THEN 'Teens'
            WHEN Age BETWEEN 20 AND 34 THEN 'Young Adults'
            WHEN Age BETWEEN 35 AND 49 THEN 'Adults'
            WHEN Age BETWEEN 50 AND 64 THEN 'Middle Aged'
            WHEN Age BETWEEN 65 AND 74 THEN 'Seniors'
            WHEN Age BETWEEN 75 AND 84 THEN 'Elderly'
            WHEN Age > 84 THEN 'Very Elderly'
        END AS Age_Group,

        CASE 
            WHEN Race IN ('other',' ') THEN 'NONE'
            ELSE Race
        END AS Race,

        CASE 
            WHEN Gender = ' ' OR Gender LIKE '%None%' THEN 'Unknown'
            ELSE Gender 
        END AS Gender,

        CASE
            WHEN Email IS NOT NULL AND Email NOT IN ('NONE',' ') THEN 1
            ELSE 0
        END AS email_flag,

        CASE
            WHEN `Social Media Handle` IS NOT NULL 
                 AND `Social Media Handle` NOT IN ('NONE',' ')
            THEN 1
            ELSE 0
        END AS SM_Flag

    FROM bright_tv_dataset.brighttv_dataset.bright_tv_userprofile
),

Viewership AS (
    SELECT
        COALESCE(userID4, UserID0) AS User_id,

        DAYNAME(RecordDate2) AS Day_name,
        HOUR(RecordDate2) AS Hour_of_Day,

        TO_CHAR(RecordDate2, 'yyyyMM') AS month_id,
        TO_DATE(RecordDate2) AS Watch_date,

        MONTHNAME(RecordDate2) AS Month_Name,

        CASE 
            WHEN DAYNAME(RecordDate2) IN ('Sat','Sun') THEN 'Weekend'
            ELSE 'Weekday'
        END AS Day_classification,

        CASE 
            WHEN Channel2 IN ('SawSee','Sawsee') THEN 'SawSee'
            WHEN Channel2 IN ('SuperSport Live Events',
                              'Live on SuperSport',
                              'DSTV Events 1')
            THEN 'Live Events'
            ELSE Channel2
        END AS TV_Channel,

        DATE_FORMAT(RecordDate2,'HH:mm:ss') AS Watch_Time,
        DATE_FORMAT(`Duration 2`,'HH:mm:ss') AS Duration,

ROUND(
    HOUR(`Duration 2`) * 60 +
    MINUTE(`Duration 2`) +
    SECOND(`Duration 2`) / 60,
    2
) AS Duration_Minute
    FROM bright_tv_dataset.brighttv_userprofile.bright_tv_dataset_viewership
)

SELECT A.*, B.*
FROM Viewership A
LEFT JOIN User_profile B
ON A.User_id = B.userid;

   
