# Subqueries and Joins
All of the questions you'll see here pull from the open source Chinook Database. You can see the ER Diagram below. 
\
![Chinook Database](https://cdn.sqlitetutorial.net/wp-content/uploads/2015/11/sqlite-sample-database-color.jpg)
\
\
**1. How many albums does the artist Led Zeppelin have?** 
\
*Code*
```SQL
SELECT COUNT(AlbumId) 
FROM albums 
WHERE ArtistId IN (Select ArtistId from artists where name = "Led Zeppelin"); 
```
*Result*
```
+----------------+
| count(AlbumId) |
+----------------+
|             14 |
+----------------+
```
\
**2. Create a list of album titles and the unit prices for the artist "Audioslave". How many records are returned?**
\
*Code*
```SQL
SELECT UnitPrice, Title, albums.AlbumId 
FROM tracks INNER JOIN albums ON tracks.AlbumId = albums.AlbumId 
WHERE ArtistId IN (SELECT ArtistId FROM artists WHERE name = "Audioslave"); 
```
*Result*
```
+-----------+--------------+---------+
| UnitPrice | Title        | AlbumId |
+-----------+--------------+---------+
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Audioslave   |      10 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
|      0.99 | Out Of Exile |      11 |
+-----------+--------------+---------+
(Output limit exceeded, 25 of 40 total rows shown)
```
\
**3. Find the first and last name of any customer who does not have an invoice. Are there any customers returned from the query?**
\
*Code*
```SQL
SELECT FirstName, Lastname 
FROM customers 
WHERE CustomerId NOT IN (SELECT CustomerId FROM Invoices); 
```
*Result*
```
+-----------+----------+
| FirstName | LastName |
+-----------+----------+
+-----------+----------+
(Zero rows)
```
\
**4. Find the total price for each album. What is the total price for the album “Big Ones”?**
\
*Code*
```SQL
SELECT SUM(UnitPrice) AS sumUnitPrice, COUNT(AlbumId) as count, AlbumId, Name, UnitPrice 
FROM tracks 
GROUP BY AlbumId 
HAVING AlbumId IN (SELECT AlbumId FROM albums WHERE title = "Big Ones") 
ORDER BY AlbumId asc; 
```
*Result*
```
+--------------+-------+---------+--------------------+-----------+
| sumUnitPrice | count | AlbumId | Name               | UnitPrice |
+--------------+-------+---------+--------------------+-----------+
|        14.85 |    15 |       5 | Livin' On The Edge |      0.99 |
+--------------+-------+---------+--------------------+-----------+
```
\
**5. How many records are created when you apply a Cartesian join to the invoice and invoice items table?**
\
*Code*
```SQL
SELECT invoices.InvoiceId 
FROM invoices CROSS JOIN invoice_items; 
```
*Result*
```
+-----------+
| InvoiceId |
+-----------+
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
|         1 |
+-----------+
(Output limit exceeded, 25 of 922880 total rows shown)
```
