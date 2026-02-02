The flow script I used is included in the file name assignment2_postgres_taxi_scheduled.yaml. I then backfilled it from 2019 Jan to 2021 Jul 2 for both green and yellow taxi.

1) I downloaded the file and extracted it and checked the file size in file properties.

2) The variable values is found in kestra output for the execution on 2020, April

3) I ran the following query on pgadmin
```SQL
SELECT COUNT(*) 
FROM public.yellow_tripdata
WHERE filename LIKE 'yellow_tripdata_2020-%';
```

4) I ran the following query on pgadmin
```SQL
SELECT COUNT(*) 
FROM public.yellow_tripdata
WHERE filename LIKE 'green_tripdata_2020-%';
```

5) I ran the following query on pgadmin
```SQL
SELECT COUNT(*) 
FROM public.green_tripdata
WHERE filename LIKE 'green_tripdata_2021-03.csv';
```
