# Analyzing data with SQL

## Exploratory data analysis

The goal of this section is to explore the tables in the database.

Finding the number of taxi rides for each taxi company for November 15-16, 2017.
* I name the resulting field trips_amount and print it along with the company_name field.
* I sort the results by the trips_amount field in descending order.

SELECT
    cabs.company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM
    cabs
    INNER JOIN trips ON cabs.cab_id = trips.cab_id
WHERE
    trips.start_ts::date BETWEEN '2017-11-15' AND '2017-11-16'
GROUP BY
    cabs.company_name
ORDER BY
    trips_amount DESC;

Finding the number of rides for every taxi company whose name contains the words "Yellow" or "Blue" for November 1-7, 2017.
* I name the resulting variable trips_amount.
* I group the results by the company_name field.

SELECT
    cabs.company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM
    trips
    INNER JOIN cabs ON trips.cab_id = cabs.cab_id
WHERE
    (cabs.company_name LIKE '%Yellow%'
    OR cabs.company_name LIKE '%Blue%')
    AND trips.start_ts::date BETWEEN '2017-11-01' AND '2017-11-07'
GROUP BY
    cabs.company_name;

In November 2017, the most popular taxi companies were Flash Cab and Taxi Affiliation Services. I will now find the number of rides for these two companies and name the resulting variable trips_amount.
* I join the rides for all other companies in the group "Other".
* I group the data by taxi company names.
* I name the field with taxi company names company.
* I sort the result in descending order by trips_amount.

SELECT
    CASE WHEN cabs.company_name NOT IN ('Flash Cab', 'Taxi Affiliation Services') THEN 'Other'
    ELSE cabs.company_name
    END AS company,
    COUNT(trips.trip_id) AS trips_amount
FROM
    trips
    INNER JOIN cabs ON trips.cab_id = cabs.cab_id
WHERE
    trips.start_ts::date BETWEEN '2017-11-01' AND '2017-11-07'
GROUP BY
    company
ORDER BY
    trips_amount DESC;

## Testing hypotheses

The goal of this section is to collect data that would help test the hypothesis that the duration of rides from the the Loop to O'Hare International Airport changes on rainy Sundays.

I start by retrieving the identifiers of the O'Hare and Loop neighborhoods from the neighborhoods table.

SELECT
    neighborhoods.neighborhood_id,
    neighborhoods.name AS name
FROM
    neighborhoods
WHERE
    name LIKE '%Hare' OR name LIKE 'Loop';

For each hour, retrieving the weather condition records from the weather_records table.
* Using the CASE operator, I break all hours into two groups: "Bad" if the description field contains the words "rain" or "storm," and "Good" for others. I name the resulting field weather_conditions.
* The final table will include two fields: date and hour (ts) and weather_conditions.

SELECT
    weather_records.ts,
    CASE WHEN weather_records.description LIKE '%rain%' OR weather_records.description LIKE '%storm%'
    THEN 'Bad'
    ELSE 'Good'
    END AS weather_conditions
FROM
    weather_records;

Printing the desired rides, their duration and the weather conditions.
* I retrieve from the trips table all the rides that started in the Loop (neighborhood_id: 50) and ended at O'Hare (neighborhood_id: 63) on a Sunday.
* I get the weather conditions for each ride using the method I applied in the previous task.
* Also, I retrieve the duration of each ride.
* I ignore rides for which data on weather conditions is not available.

SELECT
    start_ts,
    CASE WHEN weather_records.description LIKE '%rain%' OR weather_records.description LIKE '%storm%'
    THEN 'Bad'
    ELSE 'Good'
    END AS weather_conditions,
    duration_seconds
FROM
    trips
    INNER JOIN weather_records ON trips.start_ts = weather_records.ts
WHERE
    trips.pickup_location_id = 50
    AND trips.dropoff_location_id = 63
    AND EXTRACT('isodow' FROM trips.start_ts) = 6
ORDER BY
    trip_id;
