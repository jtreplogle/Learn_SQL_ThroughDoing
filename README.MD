HOW TO USE:  Using FireFox Browser with SQLite Manager; Download the Chinook Database and connect it to the SQLite Manager.  Execute requests based on the questions below.

Instructions

Ensure you have the Chinook Database and SQLite Manager Firefox add-on installed.
Create a github repo for your answers. This repo will only contain a README file in which you will write a short description of the exercise and where you can keep track of your answers.
If you have trouble opening the SQLite Manager once it has been installed, try opening firefox, selecting "customize" at the bottom of the hamberger menu at the top right of the page. You can then drag SQLite Manager onto the toolbar where it will be easily accessible.
Open SQLite Manager and select database < Connect Database and click on "Chinook_sqlite.sqlite".
Go ahead and click around a little bit to familarize yourself with the database
Optional, but helpful: draw an ERD of the Chinook database. Make sure to label primary keys, foreign keys, and indicate the type of relationship (one to one, one to many, many to many, etc) for each relationship. Use draw.io.
When you're ready to start the exercise, click the tab labeled "Execute SQL", type in your query, and click "Run SQL."
If your query is correct (i.e. it returns the data requested below) copy and paste the query to your github README. If your query doesn't return the expected results, try try again.

For each of the following exercises, provide the appropriate query. Yes, even the ones that are expressed in the form of questions. Everything from class and the Sqlite Documentation is fair game.

1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.
2. Provide a query only showing the Customers from Brazil.
3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.
4. Provide a query showing only the Employees who are Sales Agents.
5. Provide a query showing a unique list of billing countries from the Invoice table.
6. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.
7. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.
8. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?(include both the answers and the queries used to find the answers)
9. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.
10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY
11. Provide a query that includes the track name with each invoice line item.
12. Provide a query that includes the purchased track name AND artist name with each invoice line item.
13. Provide a query that shows the # of invoices per country. HINT: GROUP BY
14. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resulant table.
15. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.
16. Provide a query that shows all Invoices but includes the # of invoice line items.
17. Provide a query that shows total sales made by each sales agent.
18. Which sales agent made the most in sales in 2009? HINT: MAX
19. Which sales agent made the most in sales over all?
20. Provide a query that shows the # of customers assigned to each sales agent.
21. Provide a query that shows the total sales per country. Which country's customers spent the most?
22. Provide a query that shows the most purchased track of 2013.
23. Provide a query that shows the top 5 most purchased tracks over all.
24. Provide a query that shows the top 3 best selling artists.
25. Provide a query that shows the most purchased Media Type..

1.  
SELECT 
Customer.FirstName || ", " || Customer.LastName AS FullName,
CustomerId,
Country
FROM Customer
WHERE Country != 'USA';

2.  
SELECT 
Customer.FirstName || ", " || Customer.LastName AS FullName,
Customer.Country
FROM Customer
WHERE Country = 'Brazil';

3. 
SELECT
Customer.FirstName || " " || Customer.LastName AS FullName,
Invoice.InvoiceId,
Invoice.InvoiceDate,
Invoice.BillingCountry,
Customer.Country
FROM Customer 
INNER JOIN Invoice 
WHERE Customer.Country = 'Brazil';

4. 
SELECT 
Employee.FirstName || " " || Employee.LastName AS FullName,
Employee.title
FROM Employee
WHERE Employee.Title = 'Sales Support Agent';

5.  
SELECT 
DISTINCT
Invoice.BillingCountry
FROM Invoice
ORDER BY Invoice.BillingCountry

6.
SELECT
Employee.FirstName,
Employee.LastName, 
InvoiceId 
FROM Invoice 
INNER JOIN Customer ON Invoice.CustomerId = Customer.CustomerId 
INNER JOIN Employee on Customer.SupportRepId = Employee.EmployeeId

7.
SELECT 
Invoice.Total, 
Customer.FirstName, 
Customer.LastName, 
Customer.Country, Customer.SupportRepId 
FROM Invoice 
INNER JOIN Customer ON Invoice.CustomerId = Customer.CustomerId 
INNER JOIN Employee ON Employee.employeeId = CUstomer.SupportRepId

8.
//Dates must be in specific form (As original input).
SELECT 
Invoice.CustomerId,
Invoice.InvoiceDate,
Total
FROM Invoice
WHERE InvoiceDate BETWEEN '2009-01-01' AND '2011-12-31'

+

SELECT
SUM(Total)
FROM Invoice
WHERE InvoiceDate BETWEEN '2009-01-01' AND '2011-12-31'

9.
SELECT 
InvoiceLine.InvoiceId, 
Count(*) 
Count 
FROM InvoiceLine
WHERE InvoiceId = 37

10.
SELECT 
InvoiceLine.InvoiceId, 
Count(*) 
Count 
FROM InvoiceLine 
GROUP By InvoiceId

11.
SELECT 
InvoiceLine.InvoiceId, 
Track.Name 
FROM InvoiceLine 
JOIN Track ON InvoiceLine.TrackId = Track.TrackId

12.
SELECT 
InvoiceLine.InvoiceId, 
Track.Name, 
Artist.Name 
FROM InvoiceLine 
INNER JOIN Track ON InvoiceLine.TrackId = Track.TrackId 
INNER JOIN Album ON Track.AlbumId = Album.AlbumId 
INNER JOIN Artist ON Album.ArtistId = Artist.ArtistId

13.
SELECT 
Invoice.BillingCountry, 
Count(*) 
Count 
FROM Invoice 
GROUP BY BillingCountry

14.
SELECT 
Playlist.Name, 
Count(*) 
Count 
FROM Track 
INNER JOIN PlaylistTrack ON PlaylistTrack.TrackId = Track.TrackId 
INNER JOIN Playlist ON Playlist.PlaylistId = PlaylistTrack.PlaylistId 
GROUP BY Playlist.Name

15.
SELECT 
Track.Name, 
Album.Title, 
MediaType.Name, 
Genre.Name 
FROM Track 
INNER JOIN Album ON Track.AlbumId = Album.AlbumId 
INNER JOIN MediaType ON MediaType.MediaTypeId = Track.MediaTypeId 
INNER JOIN Genre ON Genre.GenreId = Track.GenreId

16.
SELECT 
InvoiceLine.InvoiceId, 
Count(*) 
Count 
FROM InvoiceLine 
GROUP By InvoiceId

17.
SELECT
Employee.FirstName || ", " || Employee.LastName AS FullName,  
SUM(Invoice.Total) 
FROM Employee 
INNER JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId 
INNER JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId 
GROUP BY EmployeeId

18.
SELECT 
Employee.FirstName || ", " || Employee.LastName AS FullName, 
SUM(Invoice.Total) 
FROM Employee 
INNER JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId 
INNER JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId 
GROUP BY EmployeeId

19.


20.
SELECT 
Employee.FirstName || ", " || Employee.LastName AS FullName,  
Count(*) 
FROM Customer 
INNER JOIN Employee ON Employee.EmployeeID = CUstomer.SupportRepId 
GROUP BY SupportRepId

21.
SELECT 
Invoice.BillingCountry, 
SUM(Total) 
FROM Invoice 
GROUP BY BillingCountry
