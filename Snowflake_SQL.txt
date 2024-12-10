-- Create database and schema
CREATE DATABASE IF NOT EXISTS KPI_ANALYSIS;
USE DATABASE KPI_ANALYSIS;

CREATE SCHEMA IF NOT EXISTS NETWORTH;
USE SCHEMA NETWORTH;

CREATE OR REPLACE TABLE BRONZE_DATA (
    ID INT AUTOINCREMENT PRIMARY KEY,
    Country STRING,
    AverageNetWorth FLOAT,
    AverageNetWorthTotalWealth2021 FLOAT,
    AverageNetWorthGDPPerAdult2021 FLOAT,
    AverageNetWorthWealthPerAdult2021 FLOAT,
    AverageNetWorthShareOfWorldWealth2021 FLOAT,
    CONSTRAINT FK_COUNTRY FOREIGN KEY (Country) REFERENCES COUNTRIES_CONTINENTS(Country)
);

CREATE OR REPLACE TABLE COUNTRIES_CONTINENTS (
    Continent STRING,
    Country STRING PRIMARY KEY
    
);

-- Step 2: Create a File Format
CREATE OR REPLACE FILE FORMAT CSV_FORMAT 
TYPE = 'CSV' 
FIELD_OPTIONALLY_ENCLOSED_BY = '"' 
SKIP_HEADER = 1;

-- Step 3: Create Stages for Files
CREATE OR REPLACE STAGE BRONZE_STAGE
URL = 's3://internship.academy/bronze.csv'
CREDENTIALS = (AWS_KEY_ID = '' #Add your key AWS_SECRET_KEY = ''#add secret key);

-- Step 4: Create External Stages for S3
CREATE OR REPLACE STAGE COUNTRIES_STAGE
URL = 's3://internship.academy/countries_with_continents_cleaned.csv'
CREDENTIALS = (AWS_KEY_ID = '' AWS_SECRET_KEY = '');

LIST @BRONZE_STAGE;
LIST @COUNTRIES_STAGE;

-- Step 5: Copy Data into Tables
COPY INTO BRONZE_DATA (Country, AverageNetWorth, AverageNetWorthTotalWealth2021, AverageNetWorthGDPPerAdult2021, AverageNetWorthWealthPerAdult2021)
FROM @BRONZE_STAGE
FILE_FORMAT = (FORMAT_NAME = CSV_FORMAT);

COPY INTO COUNTRIES_CONTINENTS
FROM @COUNTRIES_STAGE
FILE_FORMAT = (FORMAT_NAME = CSV_FORMAT);

SELECT * FROM COUNTRIES_CONTINENTS LIMIT 10;
SELECT * FROM BRONZE_DATA LIMIT 10;

select count(*) from BRONZE_DATA;
select count(*) from COUNTRIES_CONTINENTS;

-- KPI 1
SELECT Country, AVG(AverageNetWorth) AS AvgNetworth
FROM BRONZE_DATA
GROUP BY Country
ORDER BY AvgNetworth DESC
LIMIT 10;

--KPI 2
SELECT CC.Continent, AVG(BD.AverageNetWorth) AS AvgNetworth
FROM BRONZE_DATA BD
JOIN COUNTRIES_CONTINENTS CC ON BD.Country = CC.Country
GROUP BY CC.Continent
ORDER BY AvgNetworth DESC
LIMIT 1;

--KPI 3
SELECT Country, AVG(AverageNetWorth) AS AvgNetworth
FROM BRONZE_DATA
GROUP BY Country
ORDER BY AvgNetworth ASC
LIMIT 10;

