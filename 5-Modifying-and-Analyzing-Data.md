# Modifying and Analyzing Data
All of the questions you'll see here pull from the open source Chinook Database. You can see the ER Diagram below. 
\
![Chinook Database](https://ucde-rey.s3.amazonaws.com/DSV1015/ChinookDatabaseSchema.png)
\
\
**1. Pull a list of customer ids with the customer’s full name, and address, along with combining their city and country together. Be sure to make a space in between these two and make it UPPER CASE.** 
\
*Code*
```SQL
SELECT CustomerId, FirstName || " " || LastName AS FullName, Address, UPPER(City || " " || Country) AS CityCountry 
FROM Customers;
```
*Result*
```
+------------+-----------------------+--------------------------------------+----------------------------+
| CustomerId | FullName              | Address                              | CityCountry                |
+------------+-----------------------+--------------------------------------+----------------------------+
|          1 | Luís Gonçalves        | Av. Brigadeiro Faria Lima, 2170      | SãO JOSé DOS CAMPOS BRAZIL |
|          2 | Leonie Köhler         | Theodor-Heuss-Straße 34              | STUTTGART GERMANY          |
|          3 | François Tremblay     | 1498 rue Bélanger                    | MONTRéAL CANADA            |
|          4 | Bjørn Hansen          | Ullevålsveien 14                     | OSLO NORWAY                |
|          5 | František Wichterlová | Klanova 9/506                        | PRAGUE CZECH REPUBLIC      |
|          6 | Helena Holý           | Rilská 3174/6                        | PRAGUE CZECH REPUBLIC      |
|          7 | Astrid Gruber         | Rotenturmstraße 4, 1010 Innere Stadt | VIENNE AUSTRIA             |
|          8 | Daan Peeters          | Grétrystraat 63                      | BRUSSELS BELGIUM           |
|          9 | Kara Nielsen          | Sønder Boulevard 51                  | COPENHAGEN DENMARK         |
|         10 | Eduardo Martins       | Rua Dr. Falcão Filho, 155            | SãO PAULO BRAZIL           |
|         11 | Alexandre Rocha       | Av. Paulista, 2022                   | SãO PAULO BRAZIL           |
|         12 | Roberto Almeida       | Praça Pio X, 119                     | RIO DE JANEIRO BRAZIL      |
|         13 | Fernanda Ramos        | Qe 7 Bloco G                         | BRASíLIA BRAZIL            |
|         14 | Mark Philips          | 8210 111 ST NW                       | EDMONTON CANADA            |
|         15 | Jennifer Peterson     | 700 W Pender Street                  | VANCOUVER CANADA           |
|         16 | Frank Harris          | 1600 Amphitheatre Parkway            | MOUNTAIN VIEW USA          |
|         17 | Jack Smith            | 1 Microsoft Way                      | REDMOND USA                |
|         18 | Michelle Brooks       | 627 Broadway                         | NEW YORK USA               |
|         19 | Tim Goyer             | 1 Infinite Loop                      | CUPERTINO USA              |
|         20 | Dan Miller            | 541 Del Medio Avenue                 | MOUNTAIN VIEW USA          |
|         21 | Kathy Chase           | 801 W 4th Street                     | RENO USA                   |
|         22 | Heather Leacock       | 120 S Orange Ave                     | ORLANDO USA                |
|         23 | John Gordon           | 69 Salem Street                      | BOSTON USA                 |
|         24 | Frank Ralston         | 162 E Superior Street                | CHICAGO USA                |
|         25 | Victor Stevens        | 319 N. Frances Street                | MADISON USA                |
+------------+-----------------------+--------------------------------------+----------------------------+
(Output limit exceeded, 25 of 59 total rows shown)
```
\
**Create a new employee user id by combining the first 4 letters of the employee’s first name with the first 2 letters of the employee’s last name. Make the new field lower case and pull each individual step to show your work.**
\
*Code*
```SQL
SELECT LOWER(SUBSTR(FirstName, 1, 4) || SUBSTR(LastName, 1, 2)) AS newid, *
FROM Employees;
```
*Result*
```
+--------+------------+----------+-----------+---------------------+-----------+---------------------+---------------------+-----------------------------+------------+-------+---------+------------+-------------------+-------------------+--------------------------+
| newid  | EmployeeId | LastName | FirstName | Title               | ReportsTo | BirthDate           | HireDate            | Address                     | City       | State | Country | PostalCode | Phone             | Fax               | Email                    |
+--------+------------+----------+-----------+---------------------+-----------+---------------------+---------------------+-----------------------------+------------+-------+---------+------------+-------------------+-------------------+--------------------------+
| andrad |          1 | Adams    | Andrew    | General Manager     |      None | 1962-02-18 00:00:00 | 2002-08-14 00:00:00 | 11120 Jasper Ave NW         | Edmonton   | AB    | Canada  | T5K 2N1    | +1 (780) 428-9482 | +1 (780) 428-3457 | andrew@chinookcorp.com   |
| nanced |          2 | Edwards  | Nancy     | Sales Manager       |         1 | 1958-12-08 00:00:00 | 2002-05-01 00:00:00 | 825 8 Ave SW                | Calgary    | AB    | Canada  | T2P 2T3    | +1 (403) 262-3443 | +1 (403) 262-3322 | nancy@chinookcorp.com    |
| janepe |          3 | Peacock  | Jane      | Sales Support Agent |         2 | 1973-08-29 00:00:00 | 2002-04-01 00:00:00 | 1111 6 Ave SW               | Calgary    | AB    | Canada  | T2P 5M5    | +1 (403) 262-3443 | +1 (403) 262-6712 | jane@chinookcorp.com     |
| margpa |          4 | Park     | Margaret  | Sales Support Agent |         2 | 1947-09-19 00:00:00 | 2003-05-03 00:00:00 | 683 10 Street SW            | Calgary    | AB    | Canada  | T2P 5G3    | +1 (403) 263-4423 | +1 (403) 263-4289 | margaret@chinookcorp.com |
| stevjo |          5 | Johnson  | Steve     | Sales Support Agent |         2 | 1965-03-03 00:00:00 | 2003-10-17 00:00:00 | 7727B 41 Ave                | Calgary    | AB    | Canada  | T3B 1Y7    | 1 (780) 836-9987  | 1 (780) 836-9543  | steve@chinookcorp.com    |
| michmi |          6 | Mitchell | Michael   | IT Manager          |         1 | 1973-07-01 00:00:00 | 2003-10-17 00:00:00 | 5827 Bowness Road NW        | Calgary    | AB    | Canada  | T3B 0C5    | +1 (403) 246-9887 | +1 (403) 246-9899 | michael@chinookcorp.com  |
| robeki |          7 | King     | Robert    | IT Staff            |         6 | 1970-05-29 00:00:00 | 2004-01-02 00:00:00 | 590 Columbia Boulevard West | Lethbridge | AB    | Canada  | T1K 5N8    | +1 (403) 456-9986 | +1 (403) 456-8485 | robert@chinookcorp.com   |
| laurca |          8 | Callahan | Laura     | IT Staff            |         6 | 1968-01-09 00:00:00 | 2004-03-04 00:00:00 | 923 7 ST NW                 | Lethbridge | AB    | Canada  | T1H 1Y8    | +1 (403) 467-3351 | +1 (403) 467-8772 | laura@chinookcorp.com    |
+--------+------------+----------+-----------+---------------------+-----------+---------------------+---------------------+-----------------------------+------------+-------+---------+------------+-------------------+-------------------+--------------------------+
```
\
**3. Show a list of employees who have worked for the company for 15 or more years using the current date function. Sort by lastname ascending.**
\
*Code*
```SQL
SELECT LastName, HireDate, DATE("NOW") AS dateNow, DATE("NOW") - HireDate AS EmployeedYears  
FROM Employees
WHERE DATE("NOW") - HireDate >= 15 
ORDER BY LastName ASC; 
```
*Result*
```
+----------+---------------------+------------+----------------+
| LastName | HireDate            | dateNow    | EmployeedYears |
+----------+---------------------+------------+----------------+
| Adams    | 2002-08-14 00:00:00 | 2020-05-29 |             18 |
| Callahan | 2004-03-04 00:00:00 | 2020-05-29 |             16 |
| Edwards  | 2002-05-01 00:00:00 | 2020-05-29 |             18 |
| Johnson  | 2003-10-17 00:00:00 | 2020-05-29 |             17 |
| King     | 2004-01-02 00:00:00 | 2020-05-29 |             16 |
| Mitchell | 2003-10-17 00:00:00 | 2020-05-29 |             17 |
| Park     | 2003-05-03 00:00:00 | 2020-05-29 |             17 |
| Peacock  | 2002-04-01 00:00:00 | 2020-05-29 |             18 |
+----------+---------------------+------------+----------------+
```
\
**Find the cities with the most customers and rank in descending order.**
\
*Code*
```SQL
SELECT Count(City) as CountCity, City, CustomerId 
FROM Customers 
GROUP BY City
HAVING City IS NOT NULL
ORDER BY CountCity DESC; 
```
*Result*
```
+-----------+---------------+------------+
| CountCity | City          | CustomerId |
+-----------+---------------+------------+
|         2 | Berlin        |         38 |
|         2 | London        |         53 |
|         2 | Mountain View |         20 |
|         2 | Paris         |         40 |
|         2 | Prague        |          6 |
|         2 | São Paulo     |         11 |
|         1 | Amsterdam     |         48 |
|         1 | Bangalore     |         59 |
|         1 | Bordeaux      |         42 |
|         1 | Boston        |         23 |
|         1 | Brasília      |         13 |
|         1 | Brussels      |          8 |
|         1 | Budapest      |         45 |
|         1 | Buenos Aires  |         56 |
|         1 | Chicago       |         24 |
|         1 | Copenhagen    |          9 |
|         1 | Cupertino     |         19 |
|         1 | Delhi         |         58 |
|         1 | Dijon         |         43 |
|         1 | Dublin        |         46 |
|         1 | Edinburgh     |         54 |
|         1 | Edmonton      |         14 |
|         1 | Fort Worth    |         26 |
|         1 | Frankfurt     |         37 |
|         1 | Halifax       |         31 |
+-----------+---------------+------------+
(Output limit exceeded, 25 of 53 total rows shown)
```
\
**5. Create a new customer invoice id by combining a customer’s invoice id with their first and last name while ordering your query in the following order: firstname, lastname, and invoiceID.**
\
*Code*
```SQL
SELECT c.FirstName, c.LastName, c.CustomerId, i.InvoiceId, c.FirstName || c.LastName || i.InvoiceId AS newId 
FROM Customers c
INNER JOIN Invoices i ON i.CustomerId = c.CustomerId
ORDER BY c.FirstName, c.LastName, i.InvoiceId;
```
*Result*
```
+-----------+----------+------------+-----------+-------------------+
| FirstName | LastName | CustomerId | InvoiceId | newId             |
+-----------+----------+------------+-----------+-------------------+
| Aaron     | Mitchell |         32 |        50 | AaronMitchell50   |
| Aaron     | Mitchell |         32 |        61 | AaronMitchell61   |
| Aaron     | Mitchell |         32 |       116 | AaronMitchell116  |
| Aaron     | Mitchell |         32 |       245 | AaronMitchell245  |
| Aaron     | Mitchell |         32 |       268 | AaronMitchell268  |
| Aaron     | Mitchell |         32 |       290 | AaronMitchell290  |
| Aaron     | Mitchell |         32 |       342 | AaronMitchell342  |
| Alexandre | Rocha    |         11 |        57 | AlexandreRocha57  |
| Alexandre | Rocha    |         11 |        68 | AlexandreRocha68  |
| Alexandre | Rocha    |         11 |       123 | AlexandreRocha123 |
| Alexandre | Rocha    |         11 |       252 | AlexandreRocha252 |
| Alexandre | Rocha    |         11 |       275 | AlexandreRocha275 |
| Alexandre | Rocha    |         11 |       297 | AlexandreRocha297 |
| Alexandre | Rocha    |         11 |       349 | AlexandreRocha349 |
| Astrid    | Gruber   |          7 |        78 | AstridGruber78    |
| Astrid    | Gruber   |          7 |        89 | AstridGruber89    |
| Astrid    | Gruber   |          7 |       144 | AstridGruber144   |
| Astrid    | Gruber   |          7 |       273 | AstridGruber273   |
| Astrid    | Gruber   |          7 |       296 | AstridGruber296   |
| Astrid    | Gruber   |          7 |       318 | AstridGruber318   |
| Astrid    | Gruber   |          7 |       370 | AstridGruber370   |
| Bjørn     | Hansen   |          4 |         2 | BjørnHansen2      |
| Bjørn     | Hansen   |          4 |        24 | BjørnHansen24     |
| Bjørn     | Hansen   |          4 |        76 | BjørnHansen76     |
| Bjørn     | Hansen   |          4 |       197 | BjørnHansen197    |
+-----------+----------+------------+-----------+-------------------+
(Output limit exceeded, 25 of 412 total rows shown)
```
