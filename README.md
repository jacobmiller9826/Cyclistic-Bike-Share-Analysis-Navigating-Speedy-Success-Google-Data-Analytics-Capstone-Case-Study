# Cyclistic Bike-Share Analysis: Navigating Speedy Success
### 🚴‍♂️ Google Data Analytics Capstone Case Study
---

## 📌 Executive Summary
Cyclistic, a premier bike-share program in Chicago, operates a robust network of over 5,800 bicycles across 692 geofenced tracking stations. While historical marketing strategies focused on broad consumer awareness across all segments, financial analysis indicates that **annual members are significantly more profitable than casual riders**. 

This case study analyzes 12 months of historical trip data (~5+ million rows) to uncover how casual riders and annual members use the system differently. The data-driven insights uncovered here serve as the foundation for a targeted marketing campaign designed to convert casual riders into high-value, long-term annual members.

---

## 🏗️ The 5-Step Analytical Framework

### 1. The Business Task
* **Objective:** Identify distinct behavioral usage patterns between casual riders and annual members to formulate a targeted marketing conversion strategy.
* **Core Question:** *How do annual members and casual riders use Cyclistic bikes differently?*
* **Primary Stakeholders:** 
  * **Lily Moreno:** Director of Marketing (Responsible for executing the campaign).
  * **Cyclistic Executive Committee:** Detail-oriented leadership approving budget and strategic alignment.

### 2. The Data Source
* **Origin:** Data sourced directly via public Amazon S3 buckets containing historical trip logs from Motivate International Inc. (Chicago's Divvy bike-share network).
* **Scope & Integrity:** A full 12-month window containing over 5 million transactional records. Data fields include unique ride IDs, bike types, timestamp entries, precise geographic station parameters, and structural user classification tags. 
* **Licensing:** Data made public under an open-source data license agreement.

### 3. Data Cleaning & Feature Engineering (SQL)
To ensure processing speed and scalability over 5+ million rows, **Google BigQuery (ANSI SQL)** was utilized. 

#### Data Transformation & Cleansing Steps:
1. **Consolidation:** Stacked 12 monthly relational CSV logs using explicit `UNION ALL` partitions.
2. **Structural Stripping:** Purged rows where essential location indexes (`start_station_name`, `end_station_name`) were completely null.
3. **Anomalous Outlier Inversion:** Eliminated logical timestamps violations where `started_at` >= `ended_at`.
4. **The 1-Minute Rule:** Filtered trips under 60 seconds (system false-starts/mechanical re-docks) to avoid skewing user duration averages.
5. **The 24-Hour Cap:** Filtered trip records extending beyond 24 hours (stolen, lost, or system tracking errors).
6. **Feature Extraction:** Engineered analytical columns: `ride_length_minutes`, `ride_month`, `day_of_week`, and `start_hour`.
7. **Maintenance Purge:** Stripped Quality Assurance / HQ test indicators using specialized lower-string wildcards.

### 4. Data Analysis & Insights
The aggregated data revealed stark structural differences between user types across three critical dimensions: **Temporal, Seasonal, and Behavioral.**

#### Key Findings:
* **The Commuter vs. The Cruiser:** Annual members exhibit distinct daily spikes at **8:00 AM** and **5:00 PM** from Monday through Friday, confirming heavy utility for workplace commuting. Conversely, casual riders peak heavily on **weekends (Saturdays and Sundays)** and steady afternoon hours, indicating recreational or leisure usage.
* **Duration Disparity:** While members log higher transaction counts, casual riders ride for **more than twice as long per trip** (averaging ~24 minutes vs. members' ~12 minutes).
* **Seasonal Cohesion:** Both groups experience volume declines during Chicago's winter months (December–February), but member volumes remain far more resilient, reinforcing their dependence on the system for primary transportation.
* **Geographical Hotspots:** Top casual stations heavily anchor near leisure areas and tourist waterfront icons (e.g., Streeter Dr & Grand Ave), while member distribution is spread across inner-city transit hubs and commercial blocks.

### 5. Strategic Recommendations
Based on the data insights, a generalized marketing strategy would waste budget. Instead, we recommend three surgical initiatives:

1. **The "Weekend Warrior" Custom Membership:** Design a seasonal or weekend-only membership tier specifically targeted at casual riders who use the system regularly on Saturdays and Sundays for prolonged durations.
2. **Geo-Fenced Summer Digital Campaigns:** Launch hyper-targeted mobile advertisements and physical promo booths around the top 5 tourist/waterfront stations during peak casual hours (weekends, 1:00 PM - 4:00 PM) from May through September.
3. **The "Commute & Save" ROI Campaign:** Create targeted messaging highlighting the cost-effectiveness and health benefits of annual memberships compared to continuous single-day passes, explicitly pitching to casual riders utilizing bikes during weekday rush hours.

---

## 🛠️ Repository File Structure
```text
├── README.md               <-- Executive summary and business case documentation
├── data/                   <-- Source location credentials and data schemas 
├── scripts/
│   ├── 01_combine_data.sql <-- SQL script to combine 12 monthly CSV partitions
│   └── 02_clean_data.sql   <-- Comprehensive data cleaning & feature engineering script
├── notebooks/
│   └── aggregations.sql    <-- Granular metric summaries (hourly, weekly, station hot-spots)
└── visuals/                <-- High-resolution dashboard screenshots and interactive presentation links
```

---
*Disclaimer: The dataset used for this project is public data provided by Motivate International Inc. under its specific operational guidelines.*
