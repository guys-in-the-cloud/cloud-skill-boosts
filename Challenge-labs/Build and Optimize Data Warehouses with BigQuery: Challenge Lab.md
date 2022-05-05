# Build and Optimize Data Warehouses with BigQuery: Challenge Lab


[YouTube Video Link](https://youtu.be/XFNcDCkzCVM)

## MANUAL INSTRUCTIONS

- search -> bigquery -> in your project ID click on three dots & then create Dataset -> fill the name provided by Qwiklabs -> click create 

- in the bigquery console -> left upper side -> click + add data -> search for covid 19 government -> & then select COVID 19 Government Response public dataset -> <br>

- view dataset(this will open new tab) -> in the new bigquery tab -> in the left side search for "oxford" & select -> Broaden search to all projects. you can see like   below 

![image](https://user-images.githubusercontent.com/104570014/166146795-bc022de8-d45e-4827-beb9-1880ac6230c8.png)

## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different

## Task 1: Create a table partitioned by date
```
CREATE OR REPLACE TABLE <dataset_name>.<table_name>
PARTITION BY date
OPTIONS(
partition_expiration_days=360,
description="oxford_policy_tracker table in the COVID 19 Government Response public dataset with  an expiry time set to 90 days."
) AS
SELECT
   *
FROM
   `bigquery-public-data.covid19_govt_response.oxford_policy_tracker`
WHERE
   alpha_3_code NOT IN ('GBR', 'BRA', 'CAN','USA')
```

## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different
## Task 2: Add new columns to your table
```
ALTER TABLE <dataset_name>.<table_name>
ADD COLUMN population INT64,
ADD COLUMN country_area FLOAT64,
ADD COLUMN mobility STRUCT<
   avg_retail      FLOAT64,
   avg_grocery     FLOAT64,
   avg_parks       FLOAT64,
   avg_transit     FLOAT64,
   avg_workplace   FLOAT64,
   avg_residential FLOAT64
>
```
## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different
## Task 3: Add country population data to the population column
```
CREATE OR REPLACE TABLE <dataset_name>.pop_data_2019 AS
SELECT
  country_territory_code,
  pop_data_2019
FROM 
  `bigquery-public-data.covid19_ecdc.covid_19_geographic_distribution_worldwide`
GROUP BY
  country_territory_code,
  pop_data_2019
ORDER BY
  country_territory_code
```
## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different
## in different query tab
```
UPDATE
   `<dataset_name>.<table_name>` t0
SET
   population = t1.pop_data_2019
FROM
   `<dataset_name>.pop_data_2019` t1
WHERE
   CONCAT(t0.alpha_3_code) = CONCAT(t1.country_territory_code);
   ```
## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different  
## Task 4: Add country area data to the country_area column

```
UPDATE
   `<dataset_name>.<table_name>` t0
SET
   t0.country_area = t1.country_area
FROM
   `bigquery-public-data.census_bureau_international.country_names_area` t1
WHERE
   t0.country_name = t1.country_name
```
 ## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different  
## Task 5: Populate the mobility record data

```
UPDATE
   `<dataset_name>.<table_name>` t0
SET
   t0.mobility.avg_retail      = t1.avg_retail,
   t0.mobility.avg_grocery     = t1.avg_grocery,
   t0.mobility.avg_parks       = t1.avg_parks,
   t0.mobility.avg_transit     = t1.avg_transit,
   t0.mobility.avg_workplace   = t1.avg_workplace,
   t0.mobility.avg_residential = t1.avg_residential
FROM
   ( SELECT country_region, date,
      AVG(retail_and_recreation_percent_change_from_baseline) as avg_retail,
      AVG(grocery_and_pharmacy_percent_change_from_baseline)  as avg_grocery,
      AVG(parks_percent_change_from_baseline) as avg_parks,
      AVG(transit_stations_percent_change_from_baseline) as avg_transit,
      AVG(workplaces_percent_change_from_baseline) as avg_workplace,
      AVG(residential_percent_change_from_baseline)  as avg_residential
      FROM `bigquery-public-data.covid19_google_mobility.mobility_report`
      GROUP BY country_region, date
   ) AS t1
WHERE
   CONCAT(t0.country_name, t0.date) = CONCAT(t1.country_region, t1.date)
```
## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different
## Task 6: Query missing data in population & country_area columns

```
SELECT country_name, population
FROM `<dataset_name>.<table_name>`
WHERE population is NULL
```
```
SELECT country_name, country_area
FROM `<dataset_name>.<table_name>`
WHERE country_area IS NULL
```
```
SELECT DISTINCT country_name
FROM `<dataset_name>.<table_name>`
WHERE population is NULL
UNION ALL
SELECT DISTINCT country_name
FROM `<dataset_name>.<table_name>`
WHERE country_area IS NULL
ORDER BY country_name ASC
```
## Note - Don't forget to replace <dataset_name> <table_name>, for everyone this is different
## CONGRATULATIONS YOUR CHALLENGE LAB IS SUCCESSFULLY COMPLETED - my samples


