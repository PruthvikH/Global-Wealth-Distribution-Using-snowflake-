
# Analyzing Data of Global Wealth Distribution

## Project Overview
This project focuses on studying the distribution of wealth across the world to understand the economic disparities between countries and continents. By leveraging a robust tech stack, we analyze wealth data to generate actionable insights and visualizations.

---

## Objectives
1. Identify the **Top 10 countries** with the highest average net worth.
2. Create a new table to map **Countries** with their respective **Continents**.
3. Determine the **Continent** with the highest average net worth.
4. List the **Lowest 10 countries** by average net worth.
5. Visualize country-wise net worth distribution using a **Pie Chart**.

---

## Data Source
The dataset used in this analysis is available [here](#).  
Example data fields include:  
- Country  
- Continent  
- Total Wealth (2021)  
- GDP Per Adult (2021)  
- Wealth Per Adult (2021)  
- World Wealth Share  

---

## Workflow

1. **Data Ingestion:**
   - Source: `wealth_data.csv`
   - Format: CSV  
   - Null and duplicate values are filtered and removed.

2. **Data Storage:**
   - Processed files are uploaded to **AWS S3** using **boto3**.

3. **Data Transformation:**
   - Using **Snowflake**, create staging and dimensional schemas.
   - Create tables to analyze key metrics.

4. **Data Visualization:**
   - Generate visual reports (e.g., Pie Chart) using **Power BI**.

---

## Technology Stack
- **AWS**: For credentials and storage (IAM, S3).  
- **Snowflake**: For database and schema setup.  
- **Python**: For data preprocessing and Medallion architecture.  
- **SQL**: For querying and analysis.  
- **Power BI**: For visualizing insights.

---

## Architecture

### Design Flow:
```
Local Data Source (wealth_data.csv)
        ↓
AWS S3 Bucket (Upload using boto3)
        ↓
Snowflake External Stage (Stage Setup)
        ↓
Staging Schema (wealth_data.csv Staging)
        ↓
Dimension Schema
        ↓
KPI Analysis
```

### Key Tables:
1. `average_net_worth_by_country`  
   - Fields: Country, Continent, Total Wealth 2021, GDP Per Adult 2021, Wealth Per Adult 2021, World Wealth Share.  

2. `countries_continents`  
   - Mapping of countries to continents.

---

## Results and Insights
1. **Top 10 Countries by Average Net Worth**  
2. **New Table of Countries and Continents**  
3. **Continent with the Highest Average Net Worth**  
4. **Bottom 10 Countries by Average Net Worth**  
5. **Pie Chart** for Net Worth Distribution by Country.

---

## Steps to Reproduce

1. **Data Preprocessing:**
   - Import libraries: `pandas`, `boto3`.
   - Remove null and duplicate values.
   - Upload cleaned data to **AWS S3**.

2. **AWS Setup:**
   - Create an IAM Role with full access to S3.
   - Download and configure credentials.

3. **Snowflake Setup:**
   - Create a database `KPI_Analysis`.
   - Set up schemas and tables (`BRONZE_DATA`, `COUNTRIES_CONTINENTS`).

4. **Data Analysis:**
   - Query for KPIs:
     - Top and Bottom 10 countries by net worth.
     - Continent with the highest average net worth.
     - Generate pie chart for distribution.

---

## Outcomes
- The wealth data is successfully processed, analyzed, and stored in AWS S3 and Snowflake.
- Insights generated help visualize global wealth distribution trends.

---

## Questions & Answers
This project includes a section addressing common questions and findings during the analysis.

Notes Ensure your S3 bucket name and Snowflake configurations match those in the code. This project uses AWS and Snowflake for demonstrating a modern cloud data pipeline.
