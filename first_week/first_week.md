**Question 1. Understanding docker first run**
Answer: pip 24.3.1 

marekpapay@Marek-MacBook-Pro-2 first_week % docker run -it --entrypoint=bash python:3.12.8
root@103f028eabd7:/# pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)›
![image](https://github.com/user-attachments/assets/a806522a-5485-4c50-996e-dee5803f6397)

**Question 2. Understanding Docker networking and docker-compose**
Answer: db:5432

pgadmin goes to DB from port 80 to 5432. 

![image](https://github.com/user-attachments/assets/2a8db6b1-ec7a-47f7-a24e-15309aaff2f1)
services:
  pgdatabase:
    image: postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
    ports:
      - "5431:5432"
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
    ports:
      - "8080:80"

**Question 3. Trip Segmentation Count**

answer = 104,838; 199,013; 109,645; 27,688; 35,202

SELECT COUNT(1) AS trips_up_to_1_mile
FROM green_taxi_trips
where trip_distance <= 1

UNION ALL

SELECT COUNT(1) AS trips_from_1_up_to_3_mile
FROM green_taxi_trips
where trip_distance > 1 and trip_distance <= 3

UNION ALL

SELECT COUNT(1) AS trips_from_3_up_to_7_mile
FROM green_taxi_trips
where trip_distance > 3 and trip_distance <= 7

UNION ALL

SELECT COUNT(1) AS trips_from_7_up_to_10_mile
FROM green_taxi_trips
where trip_distance > 7 and trip_distance <= 10

UNION ALL

SELECT COUNT(1) AS trips_from_10
FROM green_taxi_trips
where trip_distance > 10

<img width="1440" alt="image" src="https://github.com/user-attachments/assets/56fe2f3b-4539-4aae-b817-05be309e242f" />

**Question 4. Longest trip for each day**

answer = 2019-10-31

-- Longest distnance in one trip
select * from green_taxi_trips where (select max(trip_distance) from green_taxi_trips ) = trip_distance;

<img width="1440" alt="image" src="https://github.com/user-attachments/assets/5f22e2bc-6f8d-415f-86b8-a7f7e1fef23a" />


**Question 5. Three biggest pickup zones**

answer = East Harlem North, East Harlem South, Morningside Heights

-- Top pickup locations with over 13,000 in total_amount for 2019-10-18
SELECT tzl."Zone", SUM(gtr."total_amount") AS total_revenue
FROM green_taxi_trips AS gtr
LEFT JOIN taxi_zone_lookup AS tzl 
ON gtr."PULocationID" = tzl."LocationID"
WHERE TO_CHAR(gtr."lpep_pickup_datetime", 'YYYY-MM-DD') = '2019-10-18'
GROUP BY tzl."Zone"
HAVING SUM(gtr."total_amount") > 13000
ORDER BY total_revenue DESC;

<img width="1440" alt="image" src="https://github.com/user-attachments/assets/267090e8-f262-4200-bf7c-5358680a551e" />


**Question 6. Largest tip**

answer = JFK Airport

-- Largest tip for drop-off zones for passengers picked up in "East Harlem North" in October 2019
SELECT tzl_dropoff."Zone" AS dropoff_zone, 
       ROUND(MAX(gtr."tip_amount")::numeric, 2) AS largest_tip_amount
FROM green_taxi_trips AS gtr
LEFT JOIN taxi_zone_lookup AS tzl_pickup 
ON gtr."PULocationID" = tzl_pickup."LocationID"
LEFT JOIN taxi_zone_lookup AS tzl_dropoff
ON gtr."DOLocationID" = tzl_dropoff."LocationID"
WHERE TO_CHAR(gtr."lpep_pickup_datetime", 'YYYY-MM') = '2019-10'
AND tzl_pickup."Zone" = 'East Harlem North'
GROUP BY tzl_dropoff."Zone"
ORDER BY largest_tip_amount DESC
LIMIT 1;

<img width="1438" alt="image" src="https://github.com/user-attachments/assets/1d7c2a99-f372-40e0-b978-acafd9951127" />

Question 7. Terraform Workflow

answer: terraform init, terraform apply -auto-approve, terraform destroy

![image](https://github.com/user-attachments/assets/862dcca0-ea68-41a8-a43c-eab04f4fe539)

![image](https://github.com/user-attachments/assets/e79562ec-3ed0-4689-92be-45a33a63fe76)



