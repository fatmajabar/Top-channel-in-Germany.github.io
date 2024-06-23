# Data Portfolio: Excel to Power BI

# Table of contents

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Mockup](#Dashboard-mockup)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration](#data-exploration-notes)
  - [Data Cleaning](#data-cleaning)
  
- [Testing](#testing)
  - [Data Quality Tests](#data-quality-tests)
- [Visualization](#visualization)
  - [Results](#results)
  - [DAX Measures](#dax-measures)
- [Analysis](#analysis)
  - [Findings](#findings)
  - [Validation](#validation)
  - [Discovery](#discovery)
- [Recommendations](#recommendations)
  - [Potential ROI](#potential-roi)
  - [Potential Courses of Actions](#potential-courses-of-actions)
- [Conclusion](#conclusion)

## Objective

- **What is the key pain point?**

  The Head of Marketing wants to find out who the top YouTubers are in 2024 to decide on which YouTubers would be best to run marketing campaigns throughout the rest of the year.

- **What is the ideal solution?**

  To create a dashboard that provides insights into top German YouTube channels in 2024 that includes their:
  - subscriber count
  - total views
  - total videos, and
  - engagement metrics

  This will help the marketing team make informed decisions about which YouTubers to collaborate with for their marketing campaigns.


## User story

As the Head of Marketing, I want to use a dashboard that analyses YouTube channel data in the Germany.

This dashboard should allow me to identify the top performing channels based on metrics like subscriber base and average views.

With this information, I can make more informed decisions about which YouTubers are right to collaborate with, and therefore maximize how effective each marketing campaign is.

## Data source

**What data is needed to achieve our objective?**
We need data on the top Germany YouTubers in 2024 that includes their

- channel names
- total subscribers
- total views
- total videos uploaded

**Where is the data coming from? The data is sourced from  (an Excel extract), [see here to find it](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download).**


## Stages

- Design
- Developement
- Testing
- Analysis

## Design

## Dashboard components required
What should the dashboard contain based on the requirements provided?<br>
**To understand what it should contain, we need to figure out what questions we need the dashboard to answer:** <br>

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?
**For now, these are some of the questions we need to answer, this may change as we progress down our analysis.**

## Dashboard mockup
What should it look like?<br>
Some of the data visuals that may be appropriate in answering our questions include:

1. Table
2. Treemap
3. Scorecards
4. Horizontal bar chart

![Dashboard Design](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/dashboard_design.png)

### Tools

| Tool             |  Purpose |
|------------------|----------|
| Excel        | Exploring the data     |
| SQL Server   | Cleaning, testing, and analyzing the data            |
| Power BI     | Visualizing the data via interactive dashboards      |
| GitHub       | Hosting the project documentation and version control|
| Mokkup AI    | Designing the wireframe/mockup of the dashboard      |


## Development
### Pseudocode

**What's the general approach in creating this solution from start to finish?**
1. Get the data
2. Explore the data in Excel
3. Load the data into SQL Server
4. Clean the data with SQL
5. Test the data with SQL
6. Visualize the data in Power BI
7. Generate the findings based on the insights
8. Write the documentation + commentary
9. Publish the data to GitHub Pages
## Data exploration notes
This is the stage where you have a scan of what's in the data, errors, inconcsistencies, bugs, weird and corrupted characters etc

**What are your initial observations with this dataset? What's caught your attention so far?**
1. There are at least 4 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data.
2. We have more data than we need, so some of these columns would need to be removed
3.  I noticed there is some null in information for the last  4 channels , I will rmove them
## Data cleaning

What do we expect the clean data to look like? (What should it contain? What contraints should we apply to it?)
**The aim is to refine our dataset to ensure it is structured and ready for analysis.**

The cleaned data should meet the following criteria and constraints:

1. Only relevant columns should be retained.
2. All data types should be appropriate for the contents of each column.
3. No column should contain null values, indicating complete data for all records.<br>

Below is a table outlining the constraints on our cleaned dataset:

| Property            | Description      |
|---------------------|------------------|
| Number of Rows      | 95             |
| Number of Columns   | 4                |

And here is a tabular representation of the expected schema for the clean data:

| Column Name      | Data Type | Nullable |
|------------------|-----------|----------|
| channel_name     | VARCHAR   | NO       |
| total_subscribers| INTEGER   | NO       |
| total_views      | INTEGER   | NO       |
| total_videos     | INTEGER   | NO       |

**What steps are needed to clean and shape the data into the desired format?**
1. Remove unnecessary columns by only selecting the ones you need
   - what column do I need ?
	 - channel_name,total_subscribers,total_views,total_videos 
2. Remove last 5 Rows

``` sql
 /*
Data cleaning steps:-
1. remove the unnecessary columns by oly selecting the ones we need
    - what column i need ?
    - [channel_name],[total_subscribers],[total_views],[total_videos]
2. I noticed there is some null in information for the last  4 channels , 
i will rmove them 

*/

--1.
 SELECT  TOP 95
CAST ([channel_name] AS varchar(200) ) AS channel_name
      ,[total_subscribers]
      ,[total_views]
      ,[total_videos]
  FROM [youtube_db].[dbo].[updated_youtube_data_germany]
  ;
2. creat view:

CREATE view view_German_youtubers_2024 as 
SELECT  TOP 95
CAST ([channel_name] AS varchar(200) ) AS channel_name
      ,[total_subscribers]
      ,[total_views]
      ,[total_videos]
  FROM [youtube_db].[dbo].[updated_youtube_data_germany]
  ;
```


## Testing
What data quality and validation checks are you going to create?
Here are the data quality tests conducted:<br>
## Row count check
``` sql
 SELECT COUNT(*) AS coount_data
   FROM [youtube_db].[dbo].[view_German_youtubers_2024];

```
### Output
![row_check](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/count_row.png)
## column count check
### SQL Query 
``` sql
 select COUNT(*) AS count_tables FROM INFORMATION_SCHEMA.COLUMNS
  WHERE TABLE_NAME = 'view_German_youtubers_2024';
```
### Output 
![column_check](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/count_columns.png)
##  data type check 
### SQL Query 
``` sql
 
 SELECT COLUMN_NAME, DATA_TYPE
FROM
  INFORMATION_SCHEMA.COLUMNS 
  WHERE TABLE_NAME = 'view_German_youtubers_2024';
```
### Output
![data_type_check](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/data_type_check.png)

## Duplicate count check  
### SQL Query 
``` sql
  

  with dup_count as (select [channel_name],
  count(*) as douplicat_coutn
  from [dbo].[view_German_youtubers_2024]
  group by [channel_name])

  select * from dup_count 
  where douplicat_coutn >1;
  ;
```
### Output
![dupicate](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/douplicate_check.png)

## Visualization
### Results
![Dashboard](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/dashboard.gif)<br>
This shows the Top Germany Youtubers in 2024 so far.

### DAX Measures
### 1. Total Subscribers (M)
```DAX
Total Subscribers (M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(sumOfSubscribers,million)

RETURN totalSubscribers

```
### 2. Total Views (B)
``` DAX
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = ROUND(sumOfTotalViews / billion, 2)

RETURN totalViews

```

### 3. Total Videos
``` DAX
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

RETURN totalVideos

```

### 4. Average Views Per Video (M)
``` DAX
Average Views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR  avgViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())

RETURN finalAvgViewsPerVideo
```
 
### 5. Subscriber Engagement Rate
``` DAX
Subscriber Engagement Rate = 
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())

RETURN subscriberEngRate
```
### 6. Views per subscriber
``` DAX
Views Per Subscriber = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews, sumOfTotalSubscribers, BLANK())

RETURN viewsPerSubscriber
```


## Analysis
### Findings

**- What did we find?**
For this analysis, we're going to focus on the questions below to get the information we need for our marketing client
- Here are the key questions we need to answer for our marketing client:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?
   
### 1. Who are the top 10 YouTubers with the most subscribers?

| Rank | Channel Name                     | Subscribers (M) |
|------|----------------------------------|-----------------|
| 1    | Kidibli (Kinder Spielzeug Kanal) | 26.40           |
| 2    | Dhruv Rathee                     | 22.40           |
| 3    | HaerteTest                       | 19.60           |
| 4    | Ice Cream Rolls                  | 12.30           |
| 5    | Pamela Reif                      | 9.97            |
| 6    | The Voice Kids                   | 8.95            |
| 7    | freekickerz                      | 8.58            |
| 8    | Rammstein Official               | 8.16            |
| 9    | COLORS                           | 7.52            |
| 10   | TikTokTunes                      | 6.79            |


### 2. Which 3 channels have uploaded the most videos?



| Channel Name | Total Videos    |
|--------------|---------------- |
| DW Español   | 41689       	 |
| DW News      | 34696   	 |
| PietSmiet    | 27810    	 |

### 3. Which 3 channels have the most views?

| Channel Name                         | Total Views    |
|--------------------------------------|----------------|
| Kidibli (Kinder Spielzeug Kanal)     | 13,513,446,607 |
| ArkivaShqip                          | 7,462,319,450  |
| Kontor.TV                            | 6,975,459,622  |

### 4. Which 3 channels have the highest average views per video?

| Channel Name          | Average Views per Video |
|-----------------------|-------------------------|
| Rammstein Official    | 37,775,905              |
| MERO                  | 29,447,396              |
| LEO ROJAS - official  | 28,832,447              |

### 5. Which 3 channels have the highest views per subscriber ratio?

| Channel Name | Total Subscribers | Views per Subscriber Ratio |
|--------------|-------------------|----------------------------|
| ArkivaShqip  | 7,462,319,450     | 1654                       |
| PietSmiet    | 3,610,318,449     | 1461                       |
| marsgizmo    | 4,068,656,742     | 1308                       |

### 6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

| Channel Name         | Subscriber Engagement Rate per Video |
|----------------------|-------------------------------------|
| Jesus Motivation     | 3,570,000                           |
| Mois D. King         | 150,714                             |
| LEO ROJAS - official | 102,222                             |

### Notes
For this analysis, we'll prioritize analysing the metrics that are important in generating the expected ROI for our marketing client,<br>
which are the YouTube channels with the most

- subscribers
- total views
- videos uploaded

### Validation
### 1. Youtubers with the most subscribers
**Calculation breakdown**
Campaign idea = product placement<br>

a. Kidibli (Kinder Spielzeug Kanal)
- Average views per video = 11,320,000
- Product cost = $5
- Potential units sold per video = 11320000 x 2% conversion rate = 226400 units sold
- Potential revenue per video = 226400 x $5 = 1,132,000
- Campaign cost (one-time fee) = $50,000
- Net profit = $1132000 - $50,000 = $1,082,000<br>

b. Dhruv Rathee
- Average views per video = 4,680,000
- Product cost = $5
- Potential units sold per video = 4,680,000 x 2% conversion rate = 93,600 units sold
- Potential revenue per video = 93,600 x $5 = $468,000
- Campaign cost (one-time fee) = $50,000
- Net profit = $468,000- $50,000 = $418,000<br>
c. Haerte Test

- Average views per video = 2,120,000
- Product cost = $5
- Potential units sold per video = 2,120,000 x 2% conversion rate = 42,400 units sold
- Potential revenue per video = 42,400 x $5 = $212,000
- Campaign cost (one-time fee) = $50,000
- Net profit = $212,000 - $50,000 = $162,000<br>
**Best option from category: Kidibli (Kinder Spielzeug Kanal)**

### SQL Query
``` sql

  /*
  1. Define the variables 
  2. create a CTE rounds the average per video 
  3. select the column that are required for the analysis 
  4. filter the results by the YouTube channels with the highest subscriber bases
  5. order by net_profit (from highest to lowest)
  */

-- 1. Define the variables 
DECLARE @conversionRate FLOAT;
DECLARE @productCost MONEY;
DECLARE @campaignCost MONEY;

-- Set the variable values
SET @conversionRate = 0.02;    -- THE conversion Rate @ 2%
SET @productCost = 5.0;        -- THE product Cost @ 5$
SET @campaignCost = 50000.0;   -- THE campaign Cost $50,000

-- 2. create a CTE rounds the average per video 
WITH ChannelData AS (
	SELECT
		[channel_name],
		[total_views],
		[total_videos],
		ROUND((CAST([total_views] AS FLOAT) / [total_videos]), -4) AS rounded_avg_views_per_video
	FROM 
		[dbo].[view_German_youtubers_2024]
)
-- 3. select the columns that are required for the analysis 
-- 4. filter the results by the YouTube channels with the highest subscriber bases
-- 5. order by net_profit (from highest to lowest)
SELECT 
	[channel_name],
	rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS Potential_Product_sales_per_video,
	(rounded_avg_views_per_video * @conversionRate * @productCost) AS Potential_Product_revenue_per_video,
	((rounded_avg_views_per_video * @conversionRate * @productCost) - @campaignCost) AS net_profit
FROM 
	ChannelData
WHERE 
	[channel_name] IN ('Kidibli (Kinder Spielzeug Kanal)', 'Dhruv Rathee', 'HaerteTest')
ORDER BY 
	net_profit DESC;

```
![image](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/top_channels_based_on_subscribe.png)

### 2. Youtubers with the most videos uploaded<br>
Calculation breakdown<br>
Campaign idea = sponsored video series

a.PietSmiet
- Average views per video = 130,000
- Product cost = $5
- Potential units sold per video = 130,000 x 2% conversion rate = 2,600 units sold
- Potential revenue per video = 2,600 x $5= $1,3000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- Net profit = $1,3000 - $55,000 = -$42000 (potential loss)<br>

b. DW News
- Average views per video = 70,000
- Product cost = $5
- Potential units sold per video = 70,000 x 2% conversion rate = 1,400 units sold
- Potential revenue per video = 1,400 x $5= $7,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- Net profit = $7,000- $55,000 = -$48000 (potential loss)

b. Yogscast

- Average views per video = 60,000
- Product cost = $5
- Potential units sold per video = 60,000x 2% conversion rate = 1,200units sold
- Potential revenue per video = 1,200 x $5= $6,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- Net profit = $6,000 - $55,000 = $49000(potential loss)<br>
**Best option from category: to not invest with any channel based on the number of videos uploaded if you want to maximize the profit<br>
, all these channels make a loss.** 
### SQL Query
```sql
/* 
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/


-- 1.
DECLARE @conversionRate FLOAT = 0.02;           -- The conversion rate @ 2%
DECLARE @productCost FLOAT = 5.0;               -- The product cost @ $5
DECLARE @campaignCostPerVideo FLOAT = 5000.0;   -- The campaign cost per video @ $5,000
DECLARE @numberOfVideos INT = 11;               -- The number of videos (11)


-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS FLOAT) / total_videos), -4) AS rounded_avg_views_per_video
    FROM 
	[dbo].[view_German_youtubers_2024]
)


-- 3.
SELECT
    channel_name,
    rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    ((rounded_avg_views_per_video * @conversionRate * @productCost) - (@campaignCostPerVideo * @numberOfVideos)) AS net_profit
FROM
    ChannelData


-- 4.
WHERE
    channel_name IN ('DW Español', 'DW News', 'PietSmiet ')


-- 5.
ORDER BY
    net_profit DESC;
```
![image](https://github.com/fatmajabar/Top-channel-in-Germany.github.io/raw/main/assets/images/basedon_viedeos.png)

### 3. Youtubers with the most views
Calculation breakdown
Campaign idea = Influencer marketing

a. Kidibli (Kinder Spielzeug Kanal)<br>

-average views per video = 11,320,000
-Product cost = $5
- Potential units sold per video = 11,320,000 x 2% conversion rate = 226,400 units sold
- Potential revenue per video = 226,400x $5 = $1,132,000
- Campaign cost (3-month contract) = $130,000
- Net profit = $1,132,000- $130,000 = $1,002,000<br>
b. Kontor.TV
-Average views per video = 2,670,000
-Product cost = $5
-Potential units sold per video = 2,670,000x 2% conversion rate = 53,400 units sold
-Potential revenue per video = 53,400 x $5 = $267,000
-Campaign cost (3-month contract) = $130,000
- Net profit = $267,000 - $130,000 = $137000<br>
c. ArkivaShqip

-Average views per video = 840,000
-Product cost = $5
-Potential units sold per video = 840,000 x 2% conversion rate =16,800 units sold
-Potential revenue per video = 16,800 x $5 = $84,000
-Campaign cost (3-month contract) = $130,000
-Net profit = $84,000- $130,000 = -$46000
**Best option from category: Kidibli (Kinder Spielzeug Kanal)**
### SQL Query
```SQL

/*
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/



-- 1.
DECLARE @conversionRate FLOAT = 0.02;        -- The conversion rate @ 2%
DECLARE @productCost MONEY = 5.0;            -- The product cost @ $5
DECLARE @campaignCost MONEY = 130000.0;      -- The campaign cost @ $130,000



-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND(CAST(total_views AS FLOAT) / total_videos, -4) AS avg_views_per_video
    FROM [dbo].[view_German_youtubers_2024]
)

-- 3.
SELECT
    channel_name,
    avg_views_per_video,
    (avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    (avg_views_per_video * @conversionRate * @productCost) - @campaignCost AS net_profit
FROM
    ChannelData


-- 4.
WHERE
    channel_name IN ('Kidibli (Kinder Spielzeug Kanal)', 'ArkivaShqip', 'Kontor.TV')
	
-- 5.
ORDER BY
    net_profit DESC;



```

### Discovery
(Your content here)

## Recommendations
### Potential ROI
(Your content here)

### Potential Courses of Actions
(Your content here)

## Conclusion
(Your content here)
