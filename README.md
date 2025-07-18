# Google Data Analytics Capstone Project: How Can a Wellness Technology Company Play It Smart?

## Introduction

Bellabeat is a high-tech manufacturer of health-focused products for women. Bellabeat designs technology that informs and inspires women around the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.

**Characters:**
* **Urška Sršen:** Bellabeat's co-founder and Chief Creative Officer
* **Sando Mur:** Mathematician and Bellabeat's co-founder; a vital member of the Bellabeat executive team
* **Bellabeat marketing analytics team:** A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat's marketing strategy.

**Products:**
* **Bellabeat app:** The Bellabeat app provides users health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their practices and make healthy decisions. The Bellabeat app connects to their line of intelligent wellness products.
* **Leaf:** Bellabeat's classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf Tracker connects to the Bellabeat app to track activity, sleep, and stress.
* **Time:** This wellness watch combines the timeless look of a classic timepiece with intelligent technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide insights into your daily wellness.
* **Spring:** This water bottle tracks daily water intake using innovative technology to ensure you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.
* **Bellabeat membership:** Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness-based on their lifestyle and goals.

In this hypothetical scenario, I am a junior data analyst working on the marketing analyst team at Bellabeat. Bellabeat's co-founders believe that analyzing smart device fitness data could help unlock new growth opportunities for the company. You have been asked to focus on one of Bellabeat's products and analyze intelligent device data to understand how consumers use their smart devices. The insights discovered will then help guide the marketing strategy for the company.

I have been encouraged to use public data that explores smart device users' daily habits, specifically, the **FitBit Fitness Tracker Data**. Analyzing FitBit fitness tracker data will help gain insights into how consumers use smart devices and discover trends for Bellabeat's marketing strategy.

## Ask

**Business Task:**
Analyze Fitbit Fitness tracker data to gain insights into how consumers use smart devices and discover trends to guide the Bellabeat marketing strategy.

**Key Questions:**
* What are some trends in smart device usage?
* How could these trends apply to Bellabeat customers?
* How could these trends help influence Bellabeat's marketing strategy?

## Prepare

In the prepare phase, we identify the data being used and its credibility.

**Data Source:**
The data used for this analysis is publicly available on [Kaggle](https://www.kaggle.com/datasets/aravlakshman/fitbit-fitness-tracker-data). This dataset contains personal fitness tracker data from 30 eligible Fitbit users who consented to submit their data, including minute-level output for physical activity, heart rate, and sleep monitoring, generated between March 12, 2016, and May 12, 2016. The data was inspired by human temporal routine behavioral analysis and pattern recognition.

For this analysis, I focused on three key datasets from the collection:
* `dailyActivity_merged.csv`
* `hourlySteps_merged.csv`
* `heartrate_seconds_merged.csv`

**Data Limitations:**
* **Timeliness:** The data was collected in 2016, making it quite old. Consumer habits, technology, and health trends may have significantly changed since then, potentially rendering some insights outdated.
* **Sample Size:** The dataset includes data from a small sample of users (33 unique IDs for daily/hourly activity, and 14 for heart rate). This small sample size is not representative of the broader fitness-tracking population or Bellabeat's target audience, limiting the generalizability of findings.
* **Bias:** The data comes from Fitbit users, who may have different demographics, motivations, or habits compared to Bellabeat's target demographic (women interested in holistic wellness).
* **Data Completeness:** While the chosen datasets are relatively complete for the selected metrics, the overall dataset from Kaggle had other files (e.g., `weightLogInfo_merged`) with very few participants, which were excluded from this analysis.

**Data Credibility (ROCCC Framework):**
* **Reliability:** The data is reliable in terms of recording quantitative fitness metrics (steps, calories, distance, heart rate, active minutes).
* **Original:** The data was collected by a third-party provider (Amazon Mechanical Turk).
* **Comprehensive:** The selected datasets cover key metrics relevant to Bellabeat's products, such as activity levels, steps, calories, and heart rate.
* **Current:** The data is from 2016, which means it is not current.
* **Cited:** The data's direct source of collection beyond "Amazon Mechanical Turk" is not explicitly cited in the Kaggle description.

## Process

In the process phase, I transformed and cleaned the data using the R programming language, primarily utilizing the `tidyverse` and `lubridate` packages.

**R Data Cleaning & Transformation Steps:**

1.  **Loading Libraries:**
    ```R
    library(tidyverse)
    library(lubridate)
    ```
2.  **Loading Data:** The three key CSV files (`dailyActivity_merged.csv`, `hourlySteps_merged.csv`, `heartrate_seconds_merged.csv`) were loaded into R dataframes (e.g., `daily_activity`, `hourly_steps`, `heartrate_seconds`).
3.  **Data Type Conversion:**
    * `ActivityDate` in `daily_activity` was converted from character to `Date` type using `mdy()`.
    * `Time` in `heartrate_seconds` and `ActivityHour` in `hourly_steps` were converted from character to `POSIXct` (datetime) type using `mdy_hms()`.
4.  **Missing Values Check:**
    * Checked for missing values in all relevant columns using `colSums(is.na())`. No missing values were found in the critical columns used for analysis.
5.  **Duplicate Rows Check & Removal:**
    * Checked for duplicate rows using `sum(duplicated())`.
    * Removed any identified duplicates using `distinct()`. No duplicate rows were found in these specific datasets.
6.  **Structure Verification:**
    * Verified the structure and data types of the dataframes after transformations using `str()`.

## Analyze

To analyze the data, various summary statistics and aggregations were performed using R to uncover trends and relationships.

**Key Analytical Steps and Insights:**

1.  **Overall Daily Activity Summary:**
    * Calculated mean and median for `TotalSteps` (Mean: 6,547; Median: 5,986), `Calories` (Mean: 2,189; Median: 2,062), and various active/sedentary minutes.
    * **Insight:** The average participant logs approximately 6,547 steps and burns around 2,189 calories daily.
2.  **Daily Activity by Day of the Week:**
    * Grouped `daily_activity` by `day_of_week` and calculated average steps, calories, and active minutes for each day.
    * **Insight:** Activity levels vary throughout the week, with Wednesdays (7,511 steps), Mondays (7,119 steps), and Saturdays (7,090 steps) showing higher average steps. Tuesdays (4,915 steps) tend to have the lowest average steps.
3.  **User Segmentation by Activity Level:**
    * Categorized users based on `SedentaryMinutes` into "Highly Sedentary" (>= 720 min), "Moderately Sedentary" (>= 480 min), and "Active/Lightly Sedentary" (below 480 min).
    * **Insight:** A significant portion of unique users are highly sedentary (35 users), followed by moderately sedentary (24 users), indicating that a large segment of users spends considerable time inactive. The average user is sedentary for approximately 995 minutes (over 16.5 hours) per day.
4.  **Average Hourly Steps:**
    * Grouped `hourly_steps` by `hour_of_day` to find average steps per hour.
    * **Insight:** There are clear peak activity hours in the day. Steps significantly increase from 6 AM, peaking in the late morning (around 9 AM - 12 PM) and again in the early evening (around 5 PM - 7 PM).
5.  **Average Heart Rate Analysis:**
    * Summarized overall `heartrate_seconds` (Mean: 79.8 bpm) and average heart rate by `hour_of_day`.
    * **Insight:** Average heart rate also shows fluctuations throughout the day, generally correlating with periods of higher activity.
6.  **Correlation Analysis:**
    * Calculated correlations: `TotalSteps` vs. `Calories` (0.58) and `TotalActiveMinutes` vs. `Calories` (0.52).
    * **Insight:** There is a moderate positive correlation between total steps/active minutes and calories burned, indicating that increased activity directly leads to higher calorie expenditure.

## Share

Data visualizations were created using `ggplot2` in R to effectively communicate the insights drawn from the analysis.

**Dashboard Highlights (Visualizations):**

Here are the key visualizations created. In a live project, these would be embedded or linked to the actual image files:

* **Average Daily Steps and Calories (Bar Chart)**
    ![Average Daily Steps and Calories](AverageDailyStepsandCalories.png)
    *Provides a high-level overview of typical daily activity.*

* **Average Daily Time in Activity Zones (Minutes) (Pie Chart)**
    ![Average Daily Time in Activity Zones](images/image_2fbbe8.png)
    *Visually emphasizes the large proportion of time users spend in sedentary activities, compared to very or fairly active minutes. (Note: Labeling issues have been addressed in the revised R code).*

* **Average Total Steps by Day of the Week (Bar Chart)**
    ![Average Total Steps by Day of the Week](images/image_2fbb83.png)
    *Clearly shows the variations in average steps across different days, highlighting more active weekdays/weekend days.*

* **User Distribution by Sedentary Activity Level (Donut Chart)**
    ![User Distribution by Sedentary Activity Level](images/WhatsApp_Image_2025-07-18_at_16.54.21_6dd36915.jpg)
    *Segments the user base, revealing that a large portion falls into highly or moderately sedentary categories.*

* **Average Steps Throughout the Day (Hourly) (Line Graph)**
    ![Average Steps Throughout the Day (Hourly)](images/WhatsApp_Image_2025-07-18_at_17.13.59_0592d00d.jpg)
    *Pinpoints the specific hours of the day when users are most active, showing morning and evening peaks.*

* **Average Heart Rate Throughout the Day (Hourly) (Line Graph)**
    ![Average Heart Rate Throughout the Day (Hourly)](images/WhatsApp_Image_2025-07-18_at_17.14.41_7f3f57f5.jpg)
    *Shows how heart rate fluctuates throughout the day, generally aligning with activity patterns.*

* **Relationship Between Total Steps and Calories Burned (Scatter Plot)**
    ![Relationship Between Total Steps and Calories Burned](images/WhatsApp_Image_2025-07-18_at_17.15.32_0d0606a7.jpg)
    *Visualizes the positive correlation, showing that more steps lead to more calories burned.*

* **Relationship Between Total Active Minutes and Calories Burned (Scatter Plot)**
    ![Relationship Between Total Active Minutes and Calories Burned](images/image_2ee608.png)
    *Further reinforces the positive relationship between overall active time and calorie expenditure.*

These visualizations confirm the trends identified in the analysis and provide a clear picture of smart device usage habits.

## Act

Keeping Bellabeat's business task and mission in mind, the insights drawn from the analysis and data visualizations can help guide their marketing strategy.

**High-Level Recommendations for Bellabeat's Marketing Strategy:**

1.  **Empower Movement to Combat Sedentary Lifestyles:**
    * **Insight:** A large segment of smart device users are highly sedentary.
    * **Recommendation:** Bellabeat should develop marketing campaigns and in-app features that specifically encourage reducing sedentary time. This could include gentle prompts for "movement breaks," guided stretching exercises for desk-bound individuals, or micro-challenges within the Bellabeat app (e.g., "Take 50 steps every hour"). Messaging should focus on empowerment and the benefits of consistent, small movements throughout the day, aligning with Bellabeat's emphasis on holistic well-being for women.

2.  **Strategize Communication Around Peak Engagement Times:**
    * **Insight:** Users exhibit clear peak activity hours (late morning and early evening) and varying activity levels throughout the week.
    * **Recommendation:** Optimize the timing of marketing communications (e.g., push notifications, email newsletters, social media posts) to coincide with these peak activity periods. Bellabeat could offer "evening wind-down" meditations or "morning motivation" tips. Campaigns can also be tailored for specific days, perhaps encouraging higher weekend activity if data shows a dip or reinforcing weekday habits.

3.  **Highlight the Holistic Bellabeat Ecosystem:**
    * **Insight:** Users are primarily tracking basic activity metrics like steps and calories.
    * **Recommendation:** Marketing efforts should emphasize Bellabeat's unique selling proposition: a comprehensive wellness ecosystem. Showcase how Leaf, Time, and Spring integrate seamlessly with the Bellabeat app to offer insights beyond just activity, covering sleep, stress, menstrual cycles, and mindfulness. This will attract users seeking a more integrated and personalized approach to their health, differentiating Bellabeat from generic fitness trackers.

4.  **Integrate Gamification and Community Features:**
    * **Insight:** Gamification and community aspects can be powerful motivators for fitness.
    * **Recommendation:** Introduce more engaging challenges, point systems, and social sharing features within the Bellabeat app. For instance, "weekly step goals," "sedentary streak breakers," or team challenges could motivate users, leveraging the positive correlations between activity and calories burned.

5.  **Cross-Promote Related Wellness Products:**
    * **Insight:** Increased activity means increased hydration needs.
    * **Recommendation:** Integrate the Bellabeat Spring water bottle into marketing campaigns focused on activity. Educate users on the crucial link between hydration and physical performance, using analytics to identify active users who could benefit most from the Spring product.
