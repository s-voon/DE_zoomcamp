# Homework 3
To begin, I created a new Google Cloud Platform (GCP) project named DE-zoomcamp and generated a service account key file (gcp.json) for authentication. Using the provided `load_yellow_taxi_data.py` script, I downloaded the required Yellow Taxi Trip Records in Parquet format and uploaded them to a Google Cloud Storage bucket named dezoomcamp_hw3_yellow_taxi_data.

To create external table using the Yellow Taxi Trip Records:
```
CREATE OR REPLACE EXTERNAL TABLE `taxi.yellow_taxi_data_external`
OPTIONS (
  format = 'Parquet',
  uris = ['gs://dezoomcamp_hw3_yellow_taxi_data/yellow_tripdata_2024-*.parquet']
  
);
```

To create a (regular/materialized) table in BQ using the Yellow Taxi Trip Records:
```
CREATE OR REPLACE TABLE taxi.yellow_taxi_data
AS
SELECT * FROM de-zoomcamp-486521.taxi.yellow_taxi_data_external;
```

## Question 1
What is count of records for the 2024 Yellow Taxi Data?
```
SELECT COUNT(*) FROM taxi.yellow_taxi_data;
```

## Question 2
Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.
```
SELECT DISTINCT(PULocationID) FROM taxi.yellow_taxi_data_external;
SELECT DISTINCT(PULocationID) FROM taxi.yellow_taxi_data;
```

### Question 3
Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. 
```
SELECT PULocationID FROM taxi.yellow_taxi_data;
```

Now write a query to retrieve the PULocationID and DOLocationID on the same table.
```
SELECT PULocationID, DOLocationID FROM taxi.yellow_taxi_data;
```

### Question 4
How many records have a fare_amount of 0?
 ```
 SELECT COUNT(*) FROM `taxi.yellow_taxi_data_external` WHERE fare_amount=0;
 ```   

 ### Question 5
 What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)
 ```
 CREATE OR REPLACE TABLE taxi.yellow_taxi_data_partitioned
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT * FROM de-zoomcamp-486521.taxi.yellow_taxi_data_external;
 ```

### Question 6 
Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive)
```
SELECT DISTINCT(VendorID) FROM `taxi.yellow_taxi_data`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';
```

```
SELECT DISTINCT(VendorID) FROM `taxi.yellow_taxi_data_partitioned`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';
```
