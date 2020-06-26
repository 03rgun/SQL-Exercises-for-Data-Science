# Profiling and Analyzing the Yelp Dataset (FINAL)
We will be using a dataset from a US-based organization called Yelp, which provides a platform for users to provide reviews and rate their interactions with a variety of organizations – businesses, restaurants, health clubs, hospitals, local governmental offices, charitable organizations, etc. Yelp has made a portion of this data available for personal, educational, and academic purposes.
\
\
The first part includes a series of questions regarding the data to help you profile and better understand the data. I will come up with my own question for analysis and will prepare a dataset for the analysis I choose to do in the second part. 
\
![Yelp Dataset ER Diagram](https://miro.medium.com/max/1644/1*FCi842EUpc1Fgfgmwdkrsw.png)

## Part 1: Yelp Dataset Profiling and Understanding
\
**1. Profile the data by finding the total number of records for each of the tables below:** 
\
*Code*
```SQL
SELECT COUNT(*) FROM [TABLE_NAME]; 
```
*Result*
```
i. Attribute table = 10000 
ii. Business table = 10000 
iii. Category table = 10000 
iv. Checkin table = 10000 
v. elite_years table = 10000 
vi. friend table = 10000
vii. hours table = 10000
viii. photo table = 10000 
ix. review table = 10000
x. tip table = 10000
xi. user table = 10000
```
\
**2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key.**
\
*Code*
```SQL
SELECT COUNT(DISTINCT [COLUMN_NAME]) FROM [TABLE_NAME]; 
```
*Result*
```
i. Business = (Primary) id: 10000 
ii. Hours = (Foreign) business_id: 1562 
iii. Category = (Foreign) business_id: 2643 
iv. Attribute = (Foreign) business_id: 1115 
v. Review = (Primary) id: 10000, (Foreign) business_id: 8090, (Foreign) user_id: 9581 
vi. Checkin = (Foreign) business_id: 493 
vii. Photo = (Primary) id: 10000, (Foreign) business_id: 6493 
viii. Tip = (Foreign) user_id: 537, (Foreign) business_id: 3979  
ix. User = (Primary) id: 10000 
x. Friend = (Foreign) user_id: 11
xi. Elite_years = (Foreign) user_id: 2780 
```
\
**3. Are there any columns with null values in the Users table?**
\
*Code*
```SQL
SELECT COUNT(*) 
	FROM user 
	WHERE 
	  id IS NULL OR 
	  name IS NULL OR
	  review_count IS NULL OR 
	  yelping_since IS NULL OR 
	  useful IS NULL OR 
	  funny IS NULL OR 
	  cool IS NULL OR 
	  fans IS NULL OR 
	  average_stars IS NULL OR 
	  compliment_hot IS NULL OR 
	  compliment_more IS NULL OR 
	  compliment_profile IS NULL OR 
	  compliment_cute IS NULL OR 
	  compliment_list IS NULL OR 
	  compliment_note IS NULL OR 
	  compliment_plain IS NULL OR 
	  compliment_cool IS NULL OR 
	  compliment_funny IS NULL OR 
	  compliment_writer IS NULL OR 
	  compliment_photos IS NULL;
```
*Result*
```
+----------+
| COUNT(*) |
+----------+
|        0 |
+----------+
```
\
**4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:**
\
*Code*
```SQL
SELECT MIN([COLUMN_NAME]), MAX([COLUMN_NAME]), AVG([COLUMN_NAME]) From [TABLE_NAME]; 
```
*Result*
```
	i. Table: Review, Column: Stars
	
		min: 1		max: 5		avg: 3.7082 
		
	
	ii. Table: Business, Column: Stars
	
		min: 1.0	max: 5.0	avg: 3.6549
		
	
	iii. Table: Tip, Column: Likes
	
		min: 0		max: 2		avg: 0.0144
		
	
	iv. Table: Checkin, Column: Count
	
		min: 1		max: 53		avg: 1.9414 
		
	
	v. Table: User, Column: Review_count
	
		min: 0		max: 2000 	avg: 24.2995
```
\
**5. List the cities with the most reviews in descending order:**
\
*Code*
```SQL
-- there are multiple records of cities with the same name so I grouped them. So I had to sum reviews. 
SELECT city, SUM(review_count) AS all_reviews 
FROM business
GROUP BY city
ORDER BY all_reviews DESC;
```
*Result*
```
	+-----------------+-------------+
	| city            | all_reviews |
	+-----------------+-------------+
	| Las Vegas       |       82854 |
	| Phoenix         |       34503 |
	| Toronto         |       24113 |
	| Scottsdale      |       20614 |
	| Charlotte       |       12523 |
	| Henderson       |       10871 |
	| Tempe           |       10504 |
	| Pittsburgh      |        9798 |
	| Montréal        |        9448 |
	| Chandler        |        8112 |
	| Mesa            |        6875 |
	| Gilbert         |        6380 |
	| Cleveland       |        5593 |
	| Madison         |        5265 |
	| Glendale        |        4406 |
	| Mississauga     |        3814 |
	| Edinburgh       |        2792 |
	| Peoria          |        2624 |
	| North Las Vegas |        2438 |
	| Markham         |        2352 |
	| Champaign       |        2029 |
	| Stuttgart       |        1849 |
	| Surprise        |        1520 |
	| Lakewood        |        1465 |
	| Goodyear        |        1155 |
	+-----------------+-------------+
	(Output limit exceeded, 25 of 362 total rows shown)
```
\
**6. Find the distribution of star ratings to the business in the following cities: Avon**
\
*Code*
```SQL
SELECT stars AS star_rating, COUNT(stars) AS count
FROM business
WHERE city = "Avon"
GROUP BY stars; 
```
*Result*
```
+-------------+-------+
| star_rating | count |
+-------------+-------+
|         1.5 |     1 |
|         2.5 |     2 |
|         3.5 |     3 |
|         4.0 |     2 |
|         4.5 |     1 |
|         5.0 |     1 |
+-------------+-------+
```
\
**7. Find the top 3 users based on their total number of reviews.**
\
*Code*
```SQL
SELECT id, name, review_count
FROM user
ORDER BY review_count DESC
LIMIT 3;		
```
*Result*
```
+------------------------+--------+--------------+
| id                     | name   | review_count |
+------------------------+--------+--------------+
| -G7Zkl1wIWBBmD0KRy_sCw | Gerald |         2000 |
| -3s52C4zL_DHRK0ULG6qtg | Sara   |         1629 |
| -8lbUNlXVSoXqaRRiHiSNg | Yuri   |         1339 |
+------------------------+--------+--------------+
```
\
**8. Does posing more reviews correlate with more fans?**
\
There is a correlation but it is very week.
\
**Please explain your findings and interpretation of the results:**
\
Well, instead of fetching fans and review count and saying there is or there is not a correlation, I wanted to calculate it properly. So I tried to apply Pearson's correlation. There was a problem though. SQLite doesn't have an built in Square root function so I just got the values I need in order to calculate correlation and moved on another another language to do it. Here is my final result: 0.62291606395669 So, there is a correlation between fans and review counts but it's not that a strong relation between them. 
\	
\
*Code*
```SQL
SELECT 
	SUM(review_count) AS x, 
	SUM(fans) AS y,
	SUM(review_count * review_count) AS x2,
	SUM(fans * fans) AS y2,
	SUM(review_count * fans) AS xy,
	COUNT(*) AS n
	-- if I would use MYSQL, I would use that function to calculate correlation: (((n * xy) - (x * y)) / (SQRT(((n * x2) - x2) * ((n * y2) - y2)))) AS correlation
FROM user;
```
*Result*
```
+--------+-------+----------+---------+---------+-------+
|      x |     y |       x2 |      y2 |      xy |     n |
+--------+-------+----------+---------+---------+-------+
| 242995 | 14896 | 62107995 | 1150280 | 5626520 | 10000 |
+--------+-------+----------+---------+---------+-------+
```
So after the calculations, the result is: 0.62291606395669
\
\
**9. Are there more reviews with the word "love" or with the word "hate" in them?**
\
*Code*
```SQL
SELECT 
	(SELECT COUNT(*) FROM review WHERE text LIKE "%love%") AS count_love,
	(SELECT COUNT(*) FROM review WHERE text LIKE "%hate%") AS count_hate;
```
*Result*
```
+------------+------------+
| count_love | count_hate |
+------------+------------+
|       1780 |        232 |
+------------+------------+
```
\
**10. Find the top 10 users with the most fans.**
\
*Code*
```SQL
SELECT id, name, fans 
FROM user 
ORDER BY fans DESC
LIMIT 10; 
```
*Result*
```
+------------------------+-----------+------+
| id                     | name      | fans |
+------------------------+-----------+------+
| -9I98YbNQnLdAmcYfb324Q | Amy       |  503 |
| -8EnCioUmDygAbsYZmTeRQ | Mimi      |  497 |
| --2vR0DIsmQ6WfcSzKWigw | Harald    |  311 |
| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |  253 |
| -0IiMAZI2SsQ7VmyzJjokQ | Christine |  173 |
| -g3XIcCb2b-BD0QBCcq2Sw | Lisa      |  159 |
| -9bbDysuiWeo2VShFJJtcw | Cat       |  133 |
| -FZBTkAZEXoP7CYvRV2ZwQ | William   |  126 |
| -9da1xk7zgnnfO1uTVYGkA | Fran      |  124 |
| -lh59ko3dxChBSZ9U7LfUw | Lissa     |  120 |
+------------------------+-----------+------+
```
## Part 2: Inferences and Analysis
\
**1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.** 
\
I've selected city as "Phoenix" and category as "Restaurants". 
\
\
**i. Do the two groups you chose to analyze have a different distribution of hours?** 
\
Yes. Restaurants that has 2-3 stars are opens at 5:00 to 11:00 and closes too late around 0:00 to 2:00. They are open like 19 hours in a day. Restaurants that has 4-5 stars are opens around 11.00 and closes way before compared to 2-3 stars restaurants. They are usually stays open around 5-9 hours in a day.  
\
**ii. Do the two groups you chose to analyze have a different number of reviews?**
\
No. In my case (selected category and city) each group has 2 businesses. Either one of them has higher and lower reviews in each group.  
\
**iii. Are you able to infer anything from the location data provided between these two groups? Explain.**
\
By looking at the address, the 4-5 stars restaruants are in East side of Phoenix. And they are all in the same state.
\
\
*Code*
```SQL
SELECT b.id, b.stars, h.hours, b.review_count, b.neighborhood, b.state,b.address,
--case part is not that necessary actually. 
	CASE 
		WHEN b.stars BETWEEN 2.0 AND 3.0 THEN "2-3 stars"
		WHEN b.stars BETWEEN 4.0 AND 5.0 THEN "4-5 stars"
		ELSE "other"
	END star_range
FROM 
	business b 
		INNER JOIN category c ON b.id = c.business_id 
		INNER JOIN hours h ON b.id = h.business_id
WHERE b.city = "Phoenix" AND c.category = "Restaurants"
GROUP BY b.stars, h.hours;
```
*Result*
```
+------------------------+-------+-----------------------+--------------+--------------+-------+-------------------------+------------+
| id                     | stars | hours                 | review_count | neighborhood | state | address                 | star_range |
+------------------------+-------+-----------------------+--------------+--------------+-------+-------------------------+------------+
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Friday|5:00-0:00      |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Monday|5:00-23:00     |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Saturday|5:00-0:00    |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Sunday|5:00-23:00     |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Thursday|5:00-23:00   |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Tuesday|5:00-23:00    |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 1Ds8V2c7LlwSAA3O-9f4cA |   2.0 | Wednesday|5:00-23:00  |            8 |              | AZ    | 1850 S 7th St           | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Friday|11:00-2:00     |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Monday|11:00-0:00     |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Saturday|9:00-2:00    |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Sunday|9:00-0:00      |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Thursday|11:00-2:00   |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Tuesday|11:00-0:00    |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 2JV0xGXsszojof2BuEt_hw |   3.0 | Wednesday|11:00-0:00  |           60 |              | AZ    | 751 E Union Hls Dr      | 2-3 stars  |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Friday|10:00-22:00    |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Monday|10:00-22:00    |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Saturday|10:00-22:00  |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Sunday|10:00-22:00    |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Thursday|10:00-22:00  |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Tuesday|10:00-22:00   |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 01xXe2m_z048W5gcBFpoJA |   3.5 | Wednesday|10:00-22:00 |           63 |              | AZ    | 2641 N 44th St, Ste 100 | other      |
| 2skQeu3C36VCiB653MIfrw |   4.0 | Friday|11:00-22:00    |          431 |              | AZ    | 3375 E Shea Blvd        | 4-5 stars  |
| 2skQeu3C36VCiB653MIfrw |   4.0 | Monday|11:00-22:00    |          431 |              | AZ    | 3375 E Shea Blvd        | 4-5 stars  |
| 2skQeu3C36VCiB653MIfrw |   4.0 | Saturday|11:00-22:00  |          431 |              | AZ    | 3375 E Shea Blvd        | 4-5 stars  |
| 2skQeu3C36VCiB653MIfrw |   4.0 | Sunday|11:00-22:00    |          431 |              | AZ    | 3375 E Shea Blvd        | 4-5 stars  |
+------------------------+-------+-----------------------+--------------+--------------+-------+-------------------------+------------+
(Output limit exceeded, 25 of 35 total rows shown)
```
\
**2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer.**
\
\
**i. Difference 1:** 
\
By looking at non-zero useful reviews; the open restaurants' average stars are slightly higher.
\
\
**ii. Difference 2:**
\
By looking at non-zero useful reviews; open restaurants' have an higher useful average rate. 
\
\
**iii. Are you able to infer anything from the location data provided between these two groups? Explain.**
\
By looking at the address, the 4-5 stars restaruants are in East side of Phoenix. And they are all in the same state.
\
\
*Code*
```SQL
SELECT COUNT(b.id) as count, AVG(b.stars), b.is_open, AVG(r.useful)
FROM business b INNER JOIN review r ON b.id = r.business_id
WHERE r.useful != 0
GROUP BY b.is_open;
```
*Result*
```
+-------+---------------+---------+---------------+
| count |  AVG(b.stars) | is_open | AVG(r.useful) |
+-------+---------------+---------+---------------+
|    34 |           3.5 |       0 | 2.02941176471 |
|   210 | 3.77380952381 |       1 | 2.30476190476 |
+-------+---------------+---------+---------------+
```
\
**3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.**
\
Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. Provide answers, in-line, to all of the following:
\
\
**i. Indicate the type of analysis you chose to do:** 
\
I wanted to look up which businesses are more succesful than others by looking at the attributes. However even though some business are in food or restraurant categories still their "GoodForMeal" values are all 0. So this query doesn't include those.
\
\
**ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:** 
\
Before starting, I included average values for all businesses for comparision. Just by looking at all businesses (first 3 columns) versus the restaurants' average values. They have better star rating than our dataset. Some groups in our dataset has 2-3 businesses so I believe it's not fair to compare them.      
I parsed json values in attribute table and created columns for each. So just by looking those, companies that serve lunch and dinner (fair amount of sample) have higher stars, reviews and they are all still open. Businesses that serves lunch only are better at stars but most of them are closed. 
So, by looking our analysis, serving lunch and dinner is the best option for businesses in food category.
\
\
*Code*
```SQL
SELECT 
  (SELECT AVG(stars) FROM business) AS star_all, 
  (SELECT AVG(is_open) FROM business) AS open_all, 
  (SELECT AVG(review_count) FROM business) AS review_all, 
  COUNT(b.id) AS count, 
  AVG(b.stars) AS avg_star, 
  AVG(b.is_open) AS avg_open, 
  AVG(review_count) as avg_review, 
  CASE
    WHEN a.value LIKE '%"dessert": true%' THEN 1 ELSE 0 
  END is_dessert,
  CASE
    WHEN a.value LIKE '%"latenight": true%' THEN 1 ELSE 0 
  END is_latenight,
  CASE
    WHEN a.value LIKE '%"lunch": true%' THEN 1 ELSE 0 
  END is_lunch,
  CASE
    WHEN a.value LIKE '%"dinner": true%' THEN 1 ELSE 0
  END is_dinner,
  CASE
    WHEN a.value LIKE '%"breakfast": true%' THEN 1 ELSE 0 
  END is_breakfast,
  CASE 
    WHEN a.value LIKE '%"brunch": true%' THEN 1 ELSE 0
  END is_brunch 
FROM business b INNER JOIN attribute a ON b.id = a.business_id
INNER JOIN category c ON b.id = c.business_id 
WHERE a.name = "GoodForMeal"
GROUP BY is_dessert, is_latenight, is_lunch, is_dinner, is_breakfast, is_brunch;
```
*Result*
```
+----------+----------+------------+-------+---------------+----------------+---------------+------------+--------------+----------+-----------+--------------+-----------+
| star_all | open_all | review_all | count |      avg_star |       avg_open |    avg_review | is_dessert | is_latenight | is_lunch | is_dinner | is_breakfast | is_brunch |
+----------+----------+------------+-------+---------------+----------------+---------------+------------+--------------+----------+-----------+--------------+-----------+
|   3.6549 |    0.848 |    30.4561 |    30 | 3.11666666667 | 0.666666666667 |           6.6 |          0 |            0 |        0 |         0 |            0 |         0 |
|   3.6549 |    0.848 |    30.4561 |     6 |           3.5 |            1.0 |          69.5 |          0 |            0 |        0 |         1 |            0 |         0 |
|   3.6549 |    0.848 |    30.4561 |    16 |        3.5625 |          0.375 |       46.1875 |          0 |            0 |        1 |         0 |            0 |         0 |
|   3.6549 |    0.848 |    30.4561 |    36 | 3.51388888889 |            1.0 | 142.611111111 |          0 |            0 |        1 |         1 |            0 |         0 |
|   3.6549 |    0.848 |    30.4561 |     3 |           3.5 |            1.0 |          83.0 |          0 |            0 |        1 |         1 |            1 |         0 |
|   3.6549 |    0.848 |    30.4561 |     2 |           2.5 |            1.0 |          28.0 |          0 |            1 |        0 |         0 |            0 |         0 |
+----------+----------+------------+-------+---------------+----------------+---------------+------------+--------------+----------+-----------+--------------+-----------+
```
