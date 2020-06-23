# Profiling and Analyzing the Yelp Dataset (FINAL)
We will be using a dataset from a US-based organization called Yelp, which provides a platform for users to provide reviews and rate their interactions with a variety of organizations – businesses, restaurants, health clubs, hospitals, local governmental offices, charitable organizations, etc. Yelp has made a portion of this data available for personal, educational, and academic purposes.
\
\
The first part includes a series of questions regarding the data to help you profile and better understand the data. I will come up with my own question for analysis and will prepare a dataset for the analysis I choose to do in the second part. 
\
![Yelp Dataset ER Diagram](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/hOlYbrgyEeeTsRKxhJ5OZg_517578844a2fd129650492eda3186cd1_YelpERDiagram.png?expiry=1593043200000&hmac=19CBdLl4W5aKRh-7rij9ZKfe63Ab7BpHmink11z2t08)

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
