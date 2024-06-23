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
  - [Transform the Data](#transform-the-data)
  - [Create the SQL View](#create-the-sql-view)
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
|| Excel        | Exploring the data     |
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
(Your content here)

### DAX Measures
(Your content here)

## Analysis
### Findings
(Your content here)

### Validation
(Your content here)

### Discovery
(Your content here)

## Recommendations
### Potential ROI
(Your content here)

### Potential Courses of Actions
(Your content here)

## Conclusion
(Your content here)
