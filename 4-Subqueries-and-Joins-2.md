# Subqueries and Joins - Part 2
All of the questions you'll see here pull from the open source Chinook Database. You can see the ER Diagram below. 
\
![Chinook Database](https://ucde-rey.s3.amazonaws.com/DSV1015/ChinookDatabaseSchema.png)
\
\
**1. Using a subquery, find the names of all the tracks for the album "Californication".** 
\
*Code*
```SQL
SELECT name 
FROM tracks 
WHERE AlbumId in (SELECT AlbumId FROM Albums WHERE Title = "Californication");
```
*Result*
```
+-------------------+
| Name              |
+-------------------+
| Around The World  |
| Parallel Universe |
| Scar Tissue       |
| Otherside         |
| Get On Top        |
| Californication   |
| Easily            |
| Porcelain         |
| Emit Remmus       |
| I Like Dirt       |
+-------------------+
(Output limit exceeded, 10 of 15 total rows shown)
```
\
**2. Find the total number of invoices for each customer along with the customer's full name, city and email.**
\
*Code*
```SQL
SELECT c.FirstName, c.LastName, c.City, COUNT(c.CustomerId) AS TotalInvoices, c.Email,  i.InvoiceId
FROM Customers AS c LEFT JOIN Invoices AS i 
ON c.CustomerId = i.CustomerID
GROUP BY c.CustomerId; 
```
*Result*
```
+-----------+-------------+---------------------+---------------+--------------------------+-----------+
| FirstName | LastName    | City                | TotalInvoices | Email                    | InvoiceId |
+-----------+-------------+---------------------+---------------+--------------------------+-----------+
| Luís      | Gonçalves   | São José dos Campos |             7 | luisg@embraer.com.br     |       382 |
| Leonie    | Köhler      | Stuttgart           |             7 | leonekohler@surfeu.de    |       293 |
| François  | Tremblay    | Montréal            |             7 | ftremblay@gmail.com      |       391 |
| Bjørn     | Hansen      | Oslo                |             7 | bjorn.hansen@yahoo.no    |       392 |
| František | Wichterlová | Prague              |             7 | frantisekw@jetbrains.com |       361 |
| Helena    | Holý        | Prague              |             7 | hholy@gmail.com          |       404 |
| Astrid    | Gruber      | Vienne              |             7 | astrid.gruber@apple.at   |       370 |
| Daan      | Peeters     | Brussels            |             7 | daan_peeters@apple.be    |       394 |
| Kara      | Nielsen     | Copenhagen          |             7 | kara.nielsen@jubii.dk    |       340 |
| Eduardo   | Martins     | São Paulo           |             7 | eduardo@woodstock.com.br |       383 |
+-----------+-------------+---------------------+---------------+--------------------------+-----------+
(Output limit exceeded, 10 of 59 total rows shown)
```
\
**3. Retrieve the track name, album, artistID, and trackID for all the albums.**
\
*Code*
```SQL
SELECT t.Name, t.albumId AS tAlbumId, t.TrackId, a.AlbumId AS aAlbumId, a.title
FROM tracks AS t INNER JOIN albums AS a 
ON t.albumId = a.albumId;
```
*Result*
```
+-----------------------------------------+----------+---------+----------+---------------------------------------+
| Name                                    | tAlbumId | TrackId | aAlbumId | Title                                 |
+-----------------------------------------+----------+---------+----------+---------------------------------------+
| For Those About To Rock (We Salute You) |        1 |       1 |        1 | For Those About To Rock We Salute You |
| Put The Finger On You                   |        1 |       6 |        1 | For Those About To Rock We Salute You |
| Let's Get It Up                         |        1 |       7 |        1 | For Those About To Rock We Salute You |
| Inject The Venom                        |        1 |       8 |        1 | For Those About To Rock We Salute You |
| Snowballed                              |        1 |       9 |        1 | For Those About To Rock We Salute You |
| Evil Walks                              |        1 |      10 |        1 | For Those About To Rock We Salute You |
| C.O.D.                                  |        1 |      11 |        1 | For Those About To Rock We Salute You |
| Breaking The Rules                      |        1 |      12 |        1 | For Those About To Rock We Salute You |
| Night Of The Long Knives                |        1 |      13 |        1 | For Those About To Rock We Salute You |
| Spellbound                              |        1 |      14 |        1 | For Those About To Rock We Salute You |
+-----------------------------------------+----------+---------+----------+---------------------------------------+
(Output limit exceeded, 10 of 3503 total rows shown)
```
\
**4. Retrieve a list with the managers last name, and the last name of the employees who report to him or her.**
\
*Code*
```SQL
SELECT A.LastName AS LastName, A.EmployeeId AS EmployeeId, A.ReportsTo AS ReportsTo, B.LastName AS EmployeesLastName, B.ReportsTo AS ManagerOf 
FROM Employees A INNER JOIN Employees B
ON a.EmployeeId = b.ReportsTo;
```
*Result*
```
+----------+------------+-----------+-------------------+-----------+
| LastName | EmployeeId | ReportsTo | EmployeesLastName | ManagerOf |
+----------+------------+-----------+-------------------+-----------+
| Adams    |          1 |      None | Edwards           |         1 |
| Adams    |          1 |      None | Mitchell          |         1 |
| Edwards  |          2 |         1 | Peacock           |         2 |
| Edwards  |          2 |         1 | Park              |         2 |
| Edwards  |          2 |         1 | Johnson           |         2 |
| Mitchell |          6 |         1 | King              |         6 |
| Mitchell |          6 |         1 | Callahan          |         6 |
+----------+------------+-----------+-------------------+-----------+
```
\
**5. Find the name and ID of the artists who do not have albums.**
\
*Code*
```SQL
SELECT name, artistId
FROM Artists 
WHERE ArtistId NOT IN (Select ArtistId from Albums);
```
*Result*
```
+----------------------------+----------+
| Name                       | ArtistId |
+----------------------------+----------+
| Milton Nascimento & Bebeto |       25 |
| Azymuth                    |       26 |
| João Gilberto              |       28 |
| Bebel Gilberto             |       29 |
| Jorge Vercilo              |       30 |
| Baby Consuelo              |       31 |
| Ney Matogrosso             |       32 |
| Luiz Melodia               |       33 |
| Nando Reis                 |       34 |
| Pedro Luís & A Parede      |       35 |
+----------------------------+----------+
(Output limit exceeded, 10 of 71 total rows shown)
```
\
**6. Use a UNION to create a list of all the employee's and customer's first names and last names ordered by the last name in descending order.**
\
*Code*
```SQL
SELECT FirstName, LastName FROM Employees
UNION
SELECT FirstName, LastName FROM Customers
ORDER BY LastName DESC; 
```
*Result*
```
+-----------+--------------+
| FirstName | LastName     |
+-----------+--------------+
| Fynn      | Zimmermann   |
| Stanisław | Wójcik       |
| František | Wichterlová  |
| Johannes  | Van der Berg |
| François  | Tremblay     |
| Mark      | Taylor       |
| Ellie     | Sullivan     |
| Victor    | Stevens      |
| Puja      | Srivastava   |
| Jack      | Smith        |
+-----------+--------------+
(Output limit exceeded, 10 of 67 total rows shown)
```
\
**7. See if there are any customers who have a different city listed in their billing city versus their customer city.**
\
*Code*
```SQL
SELECT c.city, i.BillingCity, c.CustomerId 
FROM customers as c INNER JOIN Invoices as i 
ON c.CustomerId = i.CustomerId
WHERE c.City <> i.BillingCity; 
```
*Result*
```
+------+-------------+------------+
| City | BillingCity | CustomerId |
+------+-------------+------------+
+------+-------------+------------+
(Zero rows)
```
