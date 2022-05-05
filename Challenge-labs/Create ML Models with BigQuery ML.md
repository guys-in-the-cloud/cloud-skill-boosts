# Create ML Models with BigQuery ML: Challenge Lab

[YouTube Video Link](https://youtu.be/uKXyvbhFx6o)

## Task 1: Create a dataset to store your machine learning models
```
bq mk austin
```

- Navigation Menu -> BigQuery.

## Task 2: Create a forecasting BigQuery machine learning model

In BigQuery Console Query Editor
```
CREATE OR REPLACE MODEL austin.location_model
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    EXTRACT(DAYOFWEEK FROM start_time) AS day_of_week,
    duration_minutes,
    address as location
FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
JOIN
    `bigquery-public-data.austin_bikeshare.bikeshare_stations` AS stations
ON
    trips.start_station_name = stations.name
WHERE
    EXTRACT(YEAR FROM start_time) = <replace year>
    AND duration_minutes > 0

```
## Task 3: Create the second machine learning model
- 3.1 Querry 
```
CREATE OR REPLACE MODEL austin.subscriber_model
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
WHERE EXTRACT(YEAR FROM start_time) = <replace year>
```
- 3.2 Querry
```
CREATE OR REPLACE MODEL austin.subscriber_model
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
WHERE EXTRACT(YEAR FROM start_time) = <replace year>
```
## Task 4: Evaluate the two machine learning models
- 4.1 Querry
```
-- Evaluation metrics for location_model
SELECT
  SQRT(mean_squared_error) AS rmse,
  mean_absolute_error
FROM
  ML.EVALUATE(MODEL austin.location_model, (
  SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    EXTRACT(DAYOFWEEK FROM start_time) AS day_of_week,
    duration_minutes,
    address as location
  FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
  JOIN
   `bigquery-public-data.austin_bikeshare.bikeshare_stations` AS stations
  ON
    trips.start_station_name = stations.name
  WHERE EXTRACT(YEAR FROM start_time) = <replace year> )
)


```
- 4.2 Querry 
```
-- Evaluation metrics for subscriber_model
SELECT
  SQRT(mean_squared_error) AS rmse,
  mean_absolute_error
FROM
  ML.EVALUATE(MODEL austin.subscriber_model, (
  SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
  FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
  WHERE
    EXTRACT(YEAR FROM start_time) = <replace year>)
)

```

## Task 5: Use the subscriber type machine learning model to predict average trip durations
- 5.1 Querry
```
SELECT
  start_station_name,
  COUNT(*) AS trips
FROM
  `bigquery-public-data.austin_bikeshare.bikeshare_trips`
WHERE
  EXTRACT(YEAR FROM start_time) = <replace year>
GROUP BY
  start_station_name
ORDER BY
  trips DESC

```
- 5.2 Querry
```
BigQuery Console Query Editor
SELECT AVG(predicted_duration_minutes) AS average_predicted_trip_length
FROM ML.predict(MODEL austin.subscriber_model, (
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
FROM
  `bigquery-public-data.austin_bikeshare.bikeshare_trips`
WHERE 
  EXTRACT(YEAR FROM start_time) = <replace year>
  AND subscriber_type = 'Single Trip'
  AND start_station_name = '21st & Speedway @PCL'))
  
```

# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud...

