# Filtering, Sorting and Calculating Data
All of the questions you'll see here pull from the open source Chinook Database. You can see the ER Diagram below. 
\
![Chinook Database](https://ucde-rey.s3.amazonaws.com/DSV1015/ChinookDatabaseSchema.png)
\
\
**1. Find all the tracks that have a length of 5,000,000 milliseconds or more.** 
\
*Code*
```SQL
SELECT Milliseconds 
FROM Tracks
WHERE Milliseconds >= "5000000";
```
*Result*
```
+--------------+
| Milliseconds |
+--------------+
|      5286953 |
|      5088838 |
+--------------+
```
\
**2. Find all the invoices whose total is between $5 and $15 dollars.**
\
*Code*
```SQL
SELECT total 
FROM Invoices
WHERE total BETWEEN "5" AND "15"; 
```
*Result*
```
+-------+
| Total |
+-------+
|  5.94 |
|  8.91 |
| 13.86 |
|  5.94 |
|  8.91 |
| 13.86 |
|  5.94 |
|  8.91 |
| 13.86 |
|  5.94 |
+-------+
(Output limit exceeded, 10 of 168 total rows shown)
```
\
**3. Find all the customers from the following States: RJ, DF, AB, BC, CA, WA, NY.**
\
*Code*
```SQL
SELECT FirstName, LastName, Company, State
FROM Customers
WHERE State IN ("RJ", "DF", "AB", "BC", "CA", "WA", "NY"); 
```
*Result*
```
+-----------+----------+-----------------------+-------+
| FirstName | LastName | Company               | State |
+-----------+----------+-----------------------+-------+
| Roberto   | Almeida  | Riotur                | RJ    |
| Fernanda  | Ramos    | None                  | DF    |
| Mark      | Philips  | Telus                 | AB    |
| Jennifer  | Peterson | Rogers Canada         | BC    |
| Frank     | Harris   | Google Inc.           | CA    |
| Jack      | Smith    | Microsoft Corporation | WA    |
| Michelle  | Brooks   | None                  | NY    |
| Tim       | Goyer    | Apple Inc.            | CA    |
| Dan       | Miller   | None                  | CA    |
+-----------+----------+-----------------------+-------+
```
\
**4. Find all the invoices for customer 56 and 58 where the total was between $1.00 and $5.00.**
\
*Code*
```SQL
SELECT InvoiceId, CustomerId, InvoiceDate, Total
FROM Invoices 
WHERE (CustomerId = "56" OR CustomerId = "58") AND (Total Between "1" AND "5");
```
*Result*
```
+-----------+------------+---------------------+-------+
| InvoiceId | CustomerId | InvoiceDate         | Total |
+-----------+------------+---------------------+-------+
|       119 |         56 | 2010-06-12 00:00:00 |  1.98 |
|       142 |         56 | 2010-09-14 00:00:00 |  3.96 |
|       337 |         56 | 2013-01-28 00:00:00 |  1.98 |
|       120 |         58 | 2010-06-12 00:00:00 |  1.98 |
|       315 |         58 | 2012-10-27 00:00:00 |  1.98 |
|       338 |         58 | 2013-01-29 00:00:00 |  3.96 |
|       412 |         58 | 2013-12-22 00:00:00 |  1.99 |
+-----------+------------+---------------------+-------+
```
\
**5. Find all the tracks whose name starts with 'All'.**
\
*Code*
```SQL
SELECT Name 
FROM Tracks 
WHERE Name LIKE "All%";
```
*Result*
```
+----------------------------------------+
| Name                                   |
+----------------------------------------+
| All I Really Want                      |
| All For You                            |
| All Star                               |
| All My Life                            |
| All My Love                            |
| All Within My Hands                    |
| All or None                            |
| All Dead, All Dead                     |
| All the Best Cowboys Have Daddy Issues |
| All Because Of You                     |
+----------------------------------------+
(Output limit exceeded, 10 of 15 total rows shown)
```
\
**6. Find all the customer emails that start with "J" and are from gmail.com.**
\
*Code*
```SQL
SELECT Email 
FROM Customers
WHERE Email LIKE "j%@gmail.com";
```
*Result*
```
+---------------------+
| Email               |
+---------------------+
| jubarnett@gmail.com |
+---------------------+
```
\
**7. Find all the invoices from the billing city Brasília, Edmonton, and Vancouver and sort in descending order by invoice ID.**
\
*Code*
```SQL
SELECT InvoiceId ,BillingCity, Total
FROM Invoices 
WHERE BillingCity IN ("Brasília", "Edmonton", "Vancouver")
ORDER BY InvoiceId DESC; 
```
*Result*
```
+-----------+-------------+-------+
| InvoiceId | BillingCity | Total |
+-----------+-------------+-------+
|       362 | Edmonton    | 13.86 |
|       351 | Edmonton    |  1.98 |
|       328 | Vancouver   |  0.99 |
|       319 | Brasília    |  8.91 |
|       276 | Vancouver   |  5.94 |
|       264 | Brasília    | 13.86 |
|       254 | Vancouver   |  3.96 |
|       253 | Brasília    |  1.98 |
|       231 | Vancouver   |  1.98 |
|       230 | Edmonton    |  0.99 |
+-----------+-------------+-------+
(Output limit exceeded, 10 of 21 total rows shown)
```
\
**8. Show the number of orders placed by each customer (hint: this is found in the invoices table) and sort the result by the number of orders in descending order.**
\
*Code*
```SQL
SELECT COUNT(InvoiceId) AS invoice_count, CustomerId 
FROM Invoices 
GROUP BY CustomerId
ORDER BY invoice_count DESC; 
```
*Result*
```
+---------------+------------+
| invoice_count | CustomerId |
+---------------+------------+
|             7 |          1 |
|             7 |          2 |
|             7 |          3 |
|             7 |          4 |
|             7 |          5 |
|             7 |          6 |
|             7 |          7 |
|             7 |          8 |
|             7 |          9 |
|             7 |         10 |
+---------------+------------+
(Output limit exceeded, 10 of 59 total rows shown)
```
\
**9. Find the albums with 12 or more tracks.**
\
*Code*
```SQL
SELECT AlbumId, COUNT(TrackId) AS total_tracks, Name
FROM Tracks
GROUP BY AlbumId
HAVING total_tracks >= 12;
```
*Result*
```
+---------+--------------+-----------------------------+
| AlbumId | total_tracks | Name                        |
+---------+--------------+-----------------------------+
|       5 |           15 | Livin' On The Edge          |
|       6 |           13 | You Oughta Know (Alternate) |
|       7 |           12 | Real Thing                  |
|       8 |           14 | Canta, Canta Mais           |
|      10 |           14 | The Last Remaining Light    |
|      11 |           12 | The Curse                   |
|      12 |           12 | 20 Flight Rock              |
|      14 |           13 | The Begining... At Last     |
|      18 |           17 | Freedom Of Speech           |
|      21 |           18 | Vida Boa                    |
+---------+--------------+-----------------------------+
(Output limit exceeded, 10 of 158 total rows shown)
```
