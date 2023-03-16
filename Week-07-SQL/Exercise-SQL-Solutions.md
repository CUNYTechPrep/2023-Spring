
# SQL:  Structured Query Language  Exercise


For this section of the exercise we will be using the `bigquery-public-data.new_york_311.311_service_requests`  table. 

1. Write a query that tells us how many rows are in the table. 
	```sql
	SELECT 
        COUNT(*) 
    FROM 
        `bigquery-public-data.new_york_311.311_service_requests`
	```
2. Write a query that returns all of the records for just _your_ zip code. (zipcode column is `incident_zip`)

```sql
    SELECT 
        * 
    FROM 
        `bigquery-public-data.new_york_311.311_service_requests` 
    WHERE 
        incident_zip = 11010
```

2. Lets find out what the most common complaint_type there is in NYC.  Write a query that counts all the records for each complaint_type.  Hint.. you are going to have to do a group by. 
	```sql
    SELECT
        complaint_type,
        COUNT(*) as n
    FROM
        `bigquery-public-data.new_york_311.311_service_requests`
    GROUP BY
        complaint_type
    ORDER BY 
        n DESC    
    ```
  
3. For every agency, count how many _DISTINCT_ agency_names each one has.  Hint you must use a group by for this as well.  Which agency has the most sub-agencies.  
	```sql
        SELECT
            agency,
            COUNT(DISTINCT agency_name)
        FROM
            `bigquery-public-data.new_york_311.311_service_requests`
        GROUP BY
            agency	
    ```

4. What are the 5 agency_names with the most complaints?
	```sql
	SELECT
	  agency_name,
	  COUNT(agency_name) AS n
	FROM
	  `bigquery-public-data.new_york_311.311_service_requests`
	GROUP BY
	  agency_name
	ORDER BY
	  n DESC
	LIMIT
	  5
	  ```
4. Using the `status` column Write a query that tells you how many Open cases there are.  
    ```sql
    SELECT
	  COUNT(*) AS n
	FROM
	  `bigquery-public-data.new_york_311.311_service_requests`
    WHERE 
        status = 'Open'
    ```

5. Lets inspect which complaint_type has the most open cases.  Write a query that lists and counts all the records for each complaint_type, just for the where the status is 'Open' and order the results from highest to lowest.
	```sql
	SELECT
	  complaint_type,
	  COUNT(status) AS open_cases
	FROM
	  `bigquery-public-data.new_york_311.311_service_requests`
	WHERE
	  status = 'Open'
	GROUP BY
	  complaint_type
	ORDER BY
	  open_cases DESC	
    ```


6. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function). 
	```sql
	# FIRST WAY TO DO IT USING TEMP TABLE
	WITH T AS (
	  SELECT
	      unique_key,
	      COUNT(unique_key) AS n
	    FROM
	      `bigquery-public-data.new_york_311.311_service_requests`
	    GROUP BY
	      unique_key
	  )
	SELECT * FROM T WHERE n > 1
	```

	```sql
	# SECOND WAY TO DO IT USING HAVING
		SELECT
		  unique_key,
		  COUNT(unique_key) AS n
		FROM
		  `bigquery-public-data.new_york_311.311_service_requests`
		GROUP BY
		  unique_key
		HAVING n > 1
	```


### For the next section, use the  `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table.
1. Lets find who  (`advertiser_name`) is spending the most on political ads.  Using the `advertiser_weekly_spend` table, write a query that lists the advertiser_names that spent the most in usd sort them by the amount the spent highest to lowest. 
	```sql
	SELECT
	  advertiser_name,
	  SUM(spend_usd) AS spend
	FROM
	  `bigquery-public-data.google_political_ads.advertiser_weekly_spend`
	GROUP BY
	  advertiser_name
	ORDER BY
	  spend DESC
	 ```

2. Using the same table as above, How much did the advertiser name `TOM STEYER 2020` spend in total?
	```
	8943400
	```

3. Using the same table as above, find what week_start_date had the highest spend..?
	```
	2020-10-25
	```


6.  Using the `bigquery-public-data.google_political_ads.creative_stats` how many ads the advertiser_name  'TOM STEYER 2020' campaign run?
	```
	```

5. Using the `bigquery-public-data.google_political_ads.creative_stats` table, write a query that finds which campaign (advertiser_name) ran the most ads. 
	```sql
		SELECT
		  advertiser_name,
		  COUNT(DISTINCT ad_id) AS unique_ads
		FROM
		  `bigquery-public-data.google_political_ads.creative_stats`
		GROUP BY
		  advertiser_name
		ORDER BY
		  unique_ads DESC
		LIMIT 1
	```

5. Extra credit, using the same table as above, find out how much each advertiser_name spent.  This is difficult. 




## For this next section, use the `new_york_citibike` datasets.

1. Using the `bigquery-public-data.new_york_citibike.citibike_trips` table, who went on more bike trips, self-identified Males or Females?
	```sql
	# ANSWER IS MALES
	SELECT
	  gender,
	  COUNT(*) AS n_trips
	FROM
	  `bigquery-public-data.new_york_citibike.citibike_trips`
	GROUP BY
	  gender
	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```sql
	SELECT
	  AVG(tripduration) / 60 AS avg_trip,
	  MIN(tripduration) / 60 AS min_trip,
	  MAX(tripduration) / 60 AS max_trip
	FROM
	  `bigquery-public-data.new_york_citibike.citibike_trips`
	```

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two and displays each station name, total starts, total ends and the sum total starts and total ends.) 
	```sql
    WITH
    TABLE_STARTS AS (
    SELECT
        start_station_name AS station_name,
        COUNT(*) AS n_starts
    FROM
        `bigquery-public-data.new_york_citibike.citibike_trips`
    GROUP BY
        start_station_name),
    TABLE_ENDS AS (
    SELECT
        end_station_name AS station_name,
        COUNT(*) AS n_ends
    FROM
        `bigquery-public-data.new_york_citibike.citibike_trips`
    GROUP BY
        end_station_name)
    SELECT
    *, (n_starts + n_ends) as total
    FROM
    TABLE_STARTS
    JOIN
    TABLE_ENDS
    USING
    (station_name)
	```

### For the next question, use the `census_bureau_usa` tables.

1. Write a query that returns each zipcode and their population for 2000 and 2010. Hint... You are probably going to have to use two temporary tables, then join them. 
	```sql
		WITH
		  TABLE_A AS (
		  SELECT
		    zipcode,
		    SUM(population) AS pop_2000
		  FROM
		    `bigquery-public-data.census_bureau_usa.population_by_zip_2000` 
        GROUP BY zipcode),
		  TABLE_B AS (
		  SELECT
		    zipcode,
		    SUM(population) AS pop_2010
		  FROM
		    `bigquery-public-data.census_bureau_usa.population_by_zip_2010` 
        GROUP BY zipcode)
		SELECT
		  A.zipcode,
		  A.pop_2000,
		  B.pop_2010,
		  B.pop_2010 - A.pop_2000 as pop_diff
		FROM
		  TABLE_A AS A
		JOIN
		  TABLE_B AS B
		ON
		  A.zipcode = B.zipcode ORDER BY pop_diff DESC```

## EXTRA CREDIT, using python for BigQuery

Extra credit: For the next section, open up a google colab notebook.  
1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).
2. Save a copy of it in your drive. 
3. Rename your saved version with your initials. 
4. Click the 'Share' button on the top right.  
5. Change the permissions so anyone with link can view. 
6. Copy the link and paste it right below this line. 
7. [THE LINK TO YOUR COLAB NOTEBOOK GOES HERE]
8. Begin working on the questions in that file. 
