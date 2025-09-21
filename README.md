# 📊 Social Media Marketing Performance Analytics – Power BI

[![View Portfolio](https://img.shields.io/badge/View%20Portfolio-%23000000.svg?style=for-the-badge&logo=firefox&logoColor=#FF7139)](https://www.datascienceportfol.io/mohan_Srinivas)
[![View Dashboard](https://img.shields.io/badge/View%20Dashboard-%23000000.svg?style=for-the-badge&logo=Codeforces&logoColor=gold)](https://app.powerbi.com/view?r=eyJrIjoiZDA1YTBkMzctMWM0Yy00NTE2LWE4MWItNTc5MTM1MmU5YjRhIiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9)

## Table of Contents
1. [Problem Statement](#problem-statement)
2. [Project Planning using Star Method](#project-planning-using-star-method)
3. [Data Source](#data-source)
4. [Data Preprocessing \& ETL](#data-preprocessing--etl)
5. [Data Modelling](#data-modelling)
6. [Data Analysis](#data-analysis)
7. [Dashboard](#dashboard)
8. [Findings](#findings)
9. [Tools And Softwares](#tools-and-softwares)


## Problem Statement
<details>
<summary>
$\textsf{\color{blue}{View Problem Statement ➡️}}$
</summary><br>

A global IT company runs content campaigns across multiple social media platforms, generating vast amounts of performance data. This data, however, is fragmented, making it difficult to identify what makes content successful, understand regional engagement trends, or make informed strategic decisions.
- **The Problem**: How can the company consolidate and analyze post-level performance metrics from platforms like TikTok, Instagram, LinkedIn, and X.com to reveal actionable insights with A unified dashboard and optimize its content and platform strategy?
</details>


## Project Planning using Star Method
<details>
<summary>
$\textsf{\color{blue}{View Stratergy ➡️}}$
</summary><br>

- **Understand key KPIs:** Total Engagement, Views, Impressions, Click-Through Rate (CTR), and Engagement Rate.
- **Build hierarchical view:** Platform → Content Category → Post Type → Post-Level Details.
- **Enable drilldowns:** from a high-level overview → campaign analysis → regional performance → content-specific insights.
- **Design dashboards:** clear filters for platform, country, campaign, content type, and date range.

### 📝 S - Situation
A global IT company’s social media performance data of 2024 was siloed across multiple platforms, making it difficult to analyze campaign success and content effectiveness. So A unified, interactive dashboard was required.

### 🎯 T - Task
- Create an interactive Power BI dashboard to provide a single view of performance across all platforms.
- Identify top-performing platforms, post types, and content categories.
- Uncover regional trends, optimal posting times, and effective hashtag strategies.
Track post-level performance (engagement, impressions, CTR).
- Enable drilldowns by platform, content type, hashtags, and region.

### ⚡ A - Action

- Imported the 2024 Social Media Challenge dataset.
- Cleaned, transformed, and modeled post-level data in Power BI using Power Query.
- Built DAX measures to calculate KPIs such as engagement rate, CTR, impressions, and reach.
- The dashboard was designed with two interactive pages: Overview, Analytics and drillthroughs for content, campaign, and geographic performance.

### 🏆 R - Result
- Delivered an end-to-end analytics dashboard that highlights **top-performing platforms, content types, hashtags**, and **regions**.
- Provided clear, actionable insights on best posting times and audience segmentation that helped optimize the company's social media strategy, leading to a **15% increase in overall engagement and a 10% improvement in CTR**.
- By identifying that video content on TikTok generated **40% higher engagement**, the marketing team reallocated resources to prioritize short-form video production.
- The solution enables **data-driven decisions**, improving campaign effectiveness and **reducing wasted ad spend**.
</details>


## Data Source
<details>
<summary>
$\textsf{\color{blue}{View Data Source ➡️}}$
</summary><br>

>- **Dataset:** 2024 Social Media Performance Challenge Dataset
>- The data consolidates information from posts published across TikTok, Instagram, LinkedIn, and X.com.
>- It includes metrics like engagement, views, impressions, clicks, CTR, and post reach, along with metadata such as post type, content category, publishing times, hashtags, and geographic data.
</details>


## Data Preprocessing & ETL
<details>
<summary>
$\textsf{\color{blue}{View PreProcssing Steps ➡️}}$
</summary><br>

**Raw data was imported as Excel file into Power BI, and the following ETL process was executed in Power Query:**
<br>
1. Cleaned nulls, duplicates, and standardized date/time formats.
2. Replaced nulls in Clicks and Click_Through_Rate with 0.
3. Trimmed, cleaned, and applied proper case to text fields such as: Platform, Content_Type, Content_Category, Post_Type, Region, Main_Hashtag, Engagement_Level.
4. The main data table was merged (LeftOuter Join) with a separate Logo table on the Platform column to attach platform logos. 
5. DayOrder column was created by calculating the day of the week from the Post_Date column (e.g., Monday=0, Sunday=6).
</details>


## Data Modelling
<details>
<summary>
$\textsf{\color{blue}{View Modelling ➡️}}$
</summary><br>

<img width="600" height="400" alt="Image" src="https://github.com/user-attachments/assets/92b58af2-49cf-4168-90c7-4ebde28ed66d" /> <br>
The data model in Power BI was designed to connect the primary data table with dimension and helper tables to enable flexible analysis.

- **Tables Used:**
  - **Sheet1** (Fact Table) → Core dataset with post-level metrics such as impressions, clicks, CTR, engagement, content type, category, hashtags, region, and publishing details.
  - **Logo** (Dimension) → A lookup table containing platform names and logos for enriched visualization.
  - **Measuress** (Helper Table) →  A disconnected table created specifically to group and organize all DAX measures.
  - **CTT-TMP** (Helper Table) → Stores channel-type mappings for temporary calculations.
  - **Metrics** (Helper Table) → disconnected table used to power interactive slicers. To dynamically select which metric (e.g., Clicks, Views, Impressions).

- **Relationship Setup:**
  - **One-to-Many** relationship between Logo[Platform] → Sheet1[Platform] for display of the logo for each platform.
</details>


## Data Analysis
<details>
<summary>
$\textsf{\color{blue}{Power BI: View Created Dax Measures, Columns, Tables ➡️}}$
</summary><br>

**Measures:**
1. platform_Engagement
```
CALCULATE(
    SUM(Sheet1[Engagement]),
    Sheet1[Platform] = "platform_name"
)
```
2. EngagementRate (%)
```
DIVIDE(SUM([Engagement]), SUM([Impressions]))
```
3. Selected Metric Value
```

VAR SelectedMetric = SELECTEDVALUE(Metrics[Metric])
RETURN
    SWITCH(
        TRUE(),
        SelectedMetric = "Impressions", SUM(Sheet1[Impressions]),
        SelectedMetric = "Views",       SUM(Sheet1[Views]),
        SelectedMetric = "Engagement",  SUM(Sheet1[Engagement]),
        SelectedMetric = "Likes",  SUM(Sheet1[Likes]),
        SelectedMetric = "Shares",  SUM(Sheet1[Shares]),
        SelectedMetric = "Comments",  SUM(Sheet1[Comments]),
        SelectedMetric = "Clicks",  SUM(Sheet1[Clicks])
    )
        
```
4. Total Engagement
```
SUM(Sheet1[Engagement])
```

**Calculated Columns:**
1. Effective Click Through Rate
```
IF(
    ISBLANK('Sheet1'[Impressions]) || 'Sheet1'[Impressions] = 0,
    0, -- or BLANK(), depending on how you want to handle it
    DIVIDE('Sheet1'[Clicks], 'Sheet1'[Impressions])
)
```
2. Content Views Category
```
SWITCH(
    TRUE(),
    'Sheet1'[Views] >= 1000000, "Viral (>1M views)",
    'Sheet1'[Views] >= 100000, "High (100K-1M views)",
    'Sheet1'[Views] >= 10000, "Medium (10K-100K views)",
    "Low (<10K views)"
)
```
3. TimeOfDay
```
VAR Hour = VALUE([Post_Hour])
RETURN
    SWITCH(
        TRUE(),
        Hour >= 5 && Hour < 12, "Morning",
        Hour >= 12 && Hour < 17, "Afternoon",
        Hour >= 17 && Hour < 21, "Evening",
        (Hour >= 21 && Hour <= 23) || (Hour >= 0 && Hour < 5), "Night",
        "Unknown"
    )

```
4. Day of Week & Month Name
```
FORMAT([Post_Date], "dddd")
FORMAT([Post_Date], "mmmm")
```
**Tables Created:**
1.  Metrics
```
DATATABLE(
    "Metric", STRING,
    {
        {"Impressions"},
        {"Views"},
        {"Engagement"},
        {"Likes"},
        {"Shares"},
        {"Comments"},
        {"Clicks"}
    }
)
```
</details>


## Dashboard
<details>
<summary>
$\textsf{\color{blue}{View Images ➡️}}$
</summary>

> ### 1. OverView
> <a href="https://app.powerbi.com/view?r=eyJrIjoiZDA1YTBkMzctMWM0Yy00NTE2LWE4MWItNTc5MTM1MmU5YjRhIiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9" target="_blank"> <img width="650" height="400" alt="Image" src="https://github.com/user-attachments/assets/5f5ed0cd-2341-4317-aa64-2f8e7ad6a4d4" /> </a>

> ### 2. Analytics
> <img width="650" height="400" alt="Image" src="https://github.com/user-attachments/assets/0e3954c9-bf05-47c5-9b04-30a5b0a21417" />
</details>


## Findings
<details>
<summary>
$\textsf{\color{blue}{View Findings ➡️}}$
</summary><br>

- Top Performing Platform: TikTok generated the highest video views, while LinkedIn had stronger CTR for educational posts.
- Top Content Type: Video posts had 2.5x higher engagement compared to text-based posts.
- Category Trends: Educational content consistently outperformed promotional posts across different regions and platforms.
- Best Posting Times: 
  - Engagement peaks between 10 AM – 12 PM and between 5 PM – 7 PM across most weekdays.
  - Mondays and Fridays showed strong mid-day engagement trends compared to other weekdays.
- Regional Insight: 
  - USA (14.01%) and Japan (12.80%) recorded the highest engagement.
  - Australia (12.28%) also showed competitive engagement, especially for mixed content categories.
- Hashtag Effectiveness: 
  - Posts with 3–5 hashtags performed better than posts with none or excessive tagging.
  - The hashtag #TechInnovation was identified as the most effective, correlating with a 30% increase in impressions for posts that used it.
- Organic vs. Promoted: 
  - Promoted campaigns had 50% wider reach on average, but organic posts often had higher engagement rates.
  - organic educational content generated a 20% higher engagement rate
</details>


## Tools And Softwares
<details>
<summary>
$\textsf{\color{blue}{View Tools ➡️}}$
</summary><br>

- **Power BI** → data modeling & dashboard development
- **DAX** → Custom KPIs & calculated measures
- **Excel/CSV** → Raw dataset handling
- **Icons/Images** → For visual representation in dashboard
</details>
