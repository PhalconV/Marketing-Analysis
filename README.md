# Marketing Campaign Performance Analysis

This repository contains the analysis, documentation, and results from the **Marketing Campaign Performance Analysis** project. The goal is to evaluate the effectiveness of our marketing campaigns across multiple channels, gain insights into ad performance, and optimize future strategies for better ROI.

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Objectives](#objectives)  
3. [Dataset Description](#dataset-description)
   [Tools and Technologies](#tools-and-technologies)  
5. [Key Questions Answered](#key-questions-answered)  
6. [Methodology](#methodology)  
7. [Deliverables](#deliverables)  

8. [How to Use](#how-to-use)  
9. [Acknowledgments](#acknowledgments)  

---

## Project Overview

Our company has conducted marketing campaigns across **Facebook**, **Instagram**, and **Pinterest** to promote products. This project analyzes detailed daily ad performance metrics to understand:

- Campaign effectiveness
- Channel and device performance
- Geographical trends
- Ad-level insights
- ROI trends and optimization opportunities

---

## Objectives

1. **Evaluate Campaign Performance**: Assess campaign success based on reach, engagement, and conversions.  
2. **Channel Effectiveness**: Identify the most effective channels.  
3. **Geographical Insights**: Explore city-level engagement and conversion patterns.  
4. **Device Performance**: Compare performance across mobile, desktop, and tablet devices.  
5. **Ad-Level Analysis**: Identify characteristics of high-performing ads.  
6. **ROI Calculation**: Quantify ROI for campaigns and channels.  
7. **Time Series Analysis**: Detect performance trends and seasonal effects.

---

## Dataset Description

The dataset contains the following key columns:

| Column                 | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **Campaign**           | Name of the marketing campaign (excluding null values).                    |
| **Date**               | Daily ad performance date.                                                 |
| **City/Location**      | Targeted city of the campaign.                                              |
| **Latitude**           | Latitude of the city.                                                      |
| **Longitude**          | Longitude of the city.                                                     |
| **Channel**            | Marketing channel (Facebook, Instagram, Pinterest).                        |
| **Device**             | Device used to view the ad (mobile, desktop, tablet).                      |
| **Ad**                 | Ad content identifier.                                                     |
| **Impressions**        | Number of times an ad was shown to viewers.                                |
| **CTR, %**             | Click-through rate for the ad.                                             |
| **Clicks**             | Total clicks on the ad.                                                    |
| **Daily Average CPC**  | Cost per click for the ad.                                                 |
| **Spend, GBP**         | Daily ad spend in GBP.                                                     |
| **Conversions**        | Total daily purchases attributed to an ad.                                 |
| **Total Conversion Value, GBP** | Revenue generated from purchases.                                 |
| **Likes**              | Total likes or reactions to the ad.                                        |
| **Shares**             | Total shares of the ad (Pinterest pins counted as shares).                 |
| **Comments**           | Total comments on the ad.                                                  |

---

## Tools and Technologies

- **Python**: Data cleaning, analysis, and visualization.  
- **SQL**: Data aggregation and filtering.  
- **Power BI/Tableau**: Dashboards and visual storytelling.  
- **Seaborn & Matplotlib**: Charts and graphs.  

---

## Key Questions Answered

### Campaign Performance
- Which campaign generated the highest impressions, clicks, and conversions?  
- What are the average CPC and CTR per campaign?

### Channel Effectiveness
- Which channel delivers the highest ROI?  
- How do performance metrics differ across channels?  

### Geographical Insights
- Which cities show the highest engagement (likes, shares, comments)?  
- What are the conversion rates by city?  

### Device Performance
- How do performance metrics compare across devices?  
- Which device generates the highest conversion rates?  

### Ad-Level Analysis
- What are the best-performing ads in terms of engagement and conversions?  
- What traits do successful ads share?  

### ROI and Spend Analysis
- How does ROI vary across campaigns, channels, and devices?  
- What is the correlation between ad spend and conversion value?  

### Time Series Analysis
- What are the seasonal trends in ad performance?  
- Are there any consistent patterns over time?  

---

## Methodology

1. **Data Cleaning**  
   - Removed null values and irrelevant data (e.g., campaigns with null names).  
   - Standardized formats and handled inconsistencies.  

2. **Descriptive Statistics**  
   - Summarized metrics like impressions, clicks, conversions, and CPC.  

3. **Grouping and Aggregation**  
   - Used SQL queries to group data by campaigns, channels, cities, and devices.  

4. **Visualization**  
   - Created visualizations to uncover trends, patterns, and correlations.  

5. **ROI and Conversion Analysis**  
   - Calculated ROI and analyzed spend vs. conversion value.  

6. **Reporting**  
   - Compiled insights into a detailed report with actionable recommendations.

---

## Deliverables

1. **Detailed Report**:  
   Summarizes findings, insights, and recommendations for campaign optimization.  

2. **SQL Queries**:  
   Queries used for data extraction and aggregation, including:  
   - Filtering out campaigns with null names.  
   - Grouping by channel, city, and device.  
~~~SQL
SELECT *
FROM marketing;

SELECT TOP 10 *
FROM marketing;

-- View missing or null values
SELECT *
FROM marketing
WHERE campaign IS NULL OR
	date IS NULL OR city IS NULL OR latitude IS NULL OR 
	longitude IS NULL OR channel IS NULL OR device IS NULL OR
	ad IS NULL OR impressions IS NULL OR ctr IS NULL OR clicks IS NULL OR
	daily_average_cpc IS NULL OR spend_gbp IS NULL OR conversions IS NULL OR
	total_conversion_value_gbp IS NULL OR likes IS NULL OR shares IS NULL OR coments IS NULL;

SELECT campaign,
	COUNT(impressions) AS impressions
FROM marketing
GROUP BY Campaign
ORDER BY impressions DESC;


--1.  Campaign Performance
-- ●	Which campaign generated the highest number of impressions, clicks, and conversions?
SELECT campaign,
	ROUND(SUM(impressions), 2) AS Total_Impressions,
	SUM(clicks) AS Total_Clicks,
	SUM(Conversions) AS Total_Conversions
FROM marketing
GROUP BY Campaign
ORDER BY Total_Impressions DESC;

-- ●	What is the average cost-per-click (CPC)  and click-through rate (CTR) for each campaign?
SELECT campaign,
	ROUND(AVG(daily_average_cpc), 3) AS Avg_CPC,
	ROUND(AVG(ctr), 3) AS AVG_CTR
FROM marketing
GROUP BY Campaign
ORDER BY Avg_CPC;

--2.	Channel Effectiveness:
--●	Which channel has the highest ROI?
SELECT channel,
	ROUND(SUM(total_conversion_value_gbp), 2) AS Total_Revenue,
	SUM(spend_gbp) AS Cost_of_Investment,
	ROUND(((SUM(total_conversion_value_gbp) - SUM(spend_gbp)) / SUM(spend_gbp)) * 100, 2) AS ROI_percent
FROM marketing
GROUP BY channel
ORDER BY ROI_percent DESC;

--●	How do impressions, clicks, and conversions vary across different  channels?
SELECT channel,
	ROUND(SUM(impressions), 2) AS Impressions,
	SUM(clicks) AS Clicks,
	SUM(conversions) AS Conversions
FROM marketing
GROUP BY channel
ORDER BY Impressions DESC;

--3.	Geographical Insights:
-- ●	Which cities have the highest engagement rates (likes, shares, comments)?
SELECT city,
	SUM(likes) AS Total_Likes,
	SUM(shares) AS Total_Shares,
	SUM(coments) AS Total_Comments,
	(SUM(likes) + SUM(shares) + SUM(coments)) AS Total_Engagements
FROM marketing
GROUP BY city
ORDER BY Total_Engagements DESC;

-- Engagement rate
SELECT city, ROUND(((SUM(likes) + SUM(shares) + SUM(coments)) / SUM(impressions)) * 100, 2) AS Engagement_percentage
FROM marketing
GROUP BY city;


-- I am having issues here
-- ●	What is the conversion rate by city?
SELECT city,
	SUM(conversions) AS Total_conversion,
	SUM(clicks) AS Total_clicks,
	SUM(conversions) / SUM(clicks) AS Conv
FROM marketing
GROUP BY city
ORDER BY Conv

-- 4.	Device Performance:
-- ●	How do ad performances compare across different devices (mobile, desktop, tablet)?
SELECT 
    Device,
    COUNT(*) AS Total_Ads,
    SUM(Impressions) AS Total_Impressions,
    SUM(Clicks) AS Total_Clicks,
    ROUND((SUM(Clicks) / NULLIF(SUM(Impressions), 0)) * 100, 2) AS CTR_Percentage,
    SUM(Conversions) AS Total_Conversions,
    ROUND(SUM(Total_Conversion_Value_GBP) / NULLIF(SUM(Spend_GBP), 0), 2) AS ROI 
FROM  marketing
GROUP BY Device
ORDER BY ROI DESC;

-- I am having issues deriving the conversion rate
-- ●	Which device type generates the highest conversion rates?

SELECT 
    Device,
    SUM(Conversions) AS Total_Conversions,
    SUM(Clicks) AS Total_Clicks,
    ROUND(CAST(SUM(Conversions) AS FLOAT) / NULLIF(SUM(Clicks), 0), 2) AS Conversion_Rate
FROM marketing
GROUP BY device;

-- 5.	Ad-Level Analysis:
-- ●	Which specific ads are performing best in terms of engagement and conversions?

SELECT ad,
    SUM(likes) AS Total_Likes,
    SUM(shares) AS Total_Shares,
    SUM(coments) AS Total_Comments,
    SUM(conversions) AS Total_Conversions,
    SUM(impressions) AS Total_Impressions,
    ROUND(((SUM(likes) + SUM(shares) + SUM(coments)) / NULLIF(SUM(impressions), 0)) * 100, 2) AS Engagement_Rate_Percentage,
    ROUND((SUM(conversions) / NULLIF(SUM(impressions), 0)) * 100, 2) AS Conversion_Rate_Percentage
FROM marketing
GROUP BY Ad
ORDER BY Engagement_Rate_Percentage DESC, Conversion_Rate_Percentage DESC;

-- ●	What are the common characteristics of high-performing ads?
SELECT Channel,
    Device,
    Ad,
    AVG(CAST(Likes + Shares + Coments AS FLOAT) / NULLIF(Impressions, 0) * 100) AS Avg_Engagement_Rate,
    AVG(CAST(Conversions AS FLOAT) / NULLIF(Impressions, 0) * 100) AS Avg_Conversion_Rate,
    COUNT(*) AS High_Performing_Ads
FROM marketing
WHERE (CAST(Likes + Shares + Coments AS FLOAT) / NULLIF(Impressions, 0) * 100) > 10 AND 
    (CAST(Conversions AS FLOAT) / NULLIF(Impressions, 0) * 100) > 1
GROUP BY Channel, Device, Ad
ORDER BY Avg_Engagement_Rate DESC, Avg_Conversion_Rate DESC;

--6.	ROI Calculation:
-- ●	What is the ROI for each campaign, and how does it compare across different channels and devices?
SELECT campaign, channel, device,
	ROUND(SUM(Total_conversion_value_GBP), 2) AS Total_Revenue,
	SUM(spend_GBP) AS Total_investment,
	ROUND(((SUM(Total_conversion_value_GBP) - SUM(spend_GBP)) / SUM(spend_GBP)) * 100, 2) AS ROI_percent
FROM marketing
GROUP BY Campaign, Channel, device
ORDER BY ROI_percent;

-- ●	How does spend correlate with conversion value across different campaigns?
SELECT campaign,
	SUM(spend_GBP) AS Total_Spend,
	ROUND(SUM(Total_Conversion_value_GBP), 2) AS Total_Conversion_value_GBP,
	CORR(spend_GBP, Total_conversion_value_GBP) AS Correlation_coefficient
FROM marketing
GROUP BY campaign
ORDER BY campaign;

-- 7.	Time Series Analysis:
-- ●	Are there any noticeable trends or seasonal effects in ad performance over time?
-- Daily Trends
SELECT DATENAME(WEEKDAY, date) AS DayName,
	SUM(impressions) AS Total_Impressions,
	SUM(clicks) AS Total_clicks,
	SUM(conversions) AS Total_Conversion,
	SUM(Spend_GBP) AS Total_Spend_GBP,
	(SUM(Conversions) * 1.0 / NULLIF(SUM(Clicks), 0)) AS Conversion_Rate
FROM marketing
GROUP BY DATENAME(WEEKDAY, date)
ORDER BY Conversion_Rate DESC;


-- Monthly Trends
SELECT DATENAME(MONTH, date) AS MonthName,
	SUM(impressions) AS Total_Impressions,
	SUM(clicks) AS Total_clicks,
	SUM(conversions) AS Total_Conversion,
	SUM(Spend_GBP) AS Total_Spend_GBP,
	(SUM(Conversions) * 1.0 / NULLIF(SUM(Clicks), 0)) AS Conversion_Rate
FROM marketing
GROUP BY DATENAME(MONTH, date)
ORDER BY DATENAME(MONTH, date);



SELECT TOP 5 * FROM marketing


SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'marketing';


SELECT campaign, channel, device,
	ROUND(SUM(Total_conversion_value_GBP), 2) AS Total_Revenue,
	SUM(spend_GBP) AS Total_investment,
	ROUND(((SUM(Total_conversion_value_GBP) - SUM(spend_GBP)) / SUM(spend_GBP)) * 100, 2) AS ROI_percent
FROM marketing
GROUP BY Campaign, Channel, device
ORDER BY ROI_percent;

~~~


3. **Dashboards/Visualizations**:  
   Interactive visualizations showing campaign and channel performance.  

---

