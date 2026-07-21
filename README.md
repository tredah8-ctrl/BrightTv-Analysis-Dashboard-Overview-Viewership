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

--CTE 

WITH bright_coffee_shop AS(
    SELECT transaction_id,
CASE
    WHEN transaction_qty = 1 THEN 'Single Item'
    WHEN transaction_qty = 2 THEN 'Two Items'
    ELSE 'Large Bulk'
END AS Product_Qty_Category,

CASE
    WHEN store_location = 'Lower Manhattan' THEN 'Downtown'
    WHEN store_location ILIKE 'Hells Kitchen'THEN 'Midtown'
    ELSE 'Other'
END AS Store_Region,

CASE
    WHEN product_category IN ('Coffee', 'Tea', 'Drinking Chocolate')
        THEN 'Beverages'
    WHEN product_category = 'Bakery'
        THEN 'Food'
    ELSE 'Other'
END AS Product_Group,

CASE  
      DATE_FORMAT(,'HH:mm:ss') AS Watch_Time,
        DATE_FORMAT(`Duration 2`,'HH:mm:ss') AS Duration,
ROUND(
    HOUR(`Duration 2`) * 60 +
    MINUTE(`Duration 2`) +
    SECOND(`Duration 2`) / 60,
    2
) AS Duration_Minute

CASE
    WHEN unit_price < 3 THEN 'Budget'
    WHEN unit_price BETWEEN 3 AND 4 THEN 'Standard'
    WHEN unit_price > 4 THEN 'Premium'
END AS Price_Range

FROM `workspace`.`default`.`bright_coffee_shop`
        
);

SELECT *
FROM `workspace`.`default`.`bright_coffee_shop`;



WITH bright_coffee_shop AS (
SELECT
    transaction_id,

    CASE
        WHEN transaction_qty = 1 THEN 'Single Item'
        WHEN transaction_qty = 2 THEN 'Two Items'
        ELSE 'Large Bulk'
    END AS Product_Qty_Category,

    CASE
        WHEN store_location = 'Lower Manhattan' THEN 'Downtown'
        WHEN store_location ILIKE 'Hells Kitchen' THEN 'Midtown'
        ELSE 'Other'
    END AS Store_Region,

    CASE
        WHEN product_category IN ('Coffee','Tea','Drinking Chocolate') THEN 'Beverages'
        WHEN product_category = 'Bakery' THEN 'Food'
        ELSE 'Other'
    END AS Product_Group,
CASE 

    WHEN dayname(transaction_time ) IN ('Sat','Sun') THEN 'Weekend'
    ELSE 'Weekday'
END AS Weekday,

CASE 
    WHEN HOUR(transaction_time) BETWEEN 00 AND 08 THEN 'Morning' 
            WHEN HOUR(transaction_time) BETWEEN 08 AND 16 THEN 'Afternoon'
            WHEN HOUR(transaction_time) BETWEEN 17 AND 20 THEN 'Evening'
            ELSE 'Night'
        END AS Time_Classification,

    CASE
        WHEN unit_price < 3 THEN 'Budget'
        WHEN unit_price BETWEEN 3 AND 4 THEN 'Standard'
        ELSE 'Premium'
    END AS Price_Range

FROM `workspace`.`default`.`bright_coffee_shop`
)

SELECT *
FROM bright_coffee_shop;
   
