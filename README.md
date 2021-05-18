      ### SQL WEEK 4 Coding Assignment###
    
# Q1 Pull a list of customer ids with the customer’s full name, and address, along with combining their city and country together. Be sure to make a space in between these two and make it UPPER CASE. (e.g. LOS ANGELES USA)

# V1
 SELECT CustomerId, FirstName || LastName AS FullName, UPPER(City ||" " || Country) AS Address
FROM Customers 
    
# Q2 
Create a new employee user id by combining the first 4 letters of the employee’s first name
 with the first 2 letters of the employee’s last name. 
 Make the new field lower case and pull each individual step to show your work. 
 
 # V1 
SELECT FirstName, LastName,LOWER(SUBSTR(FirstName,1,4)||(SUBSTR(LastName,1,2))) AS UserId
From Employees    
    
 # Q3
 Show a list of employees who have worked for the company for 15 or more years
 using the current date function.
 Sort by lastname ascending.
 
 # V1
 SELECT LastName, Hiredate,SUBSTR(Hiredate,1,4)AS Start,Strftime('%Y','Now')AS Year
FROM Employees
WHERE (Year-Start) >= 15
ORDER BY LastName ASC;
# V2
SELECT LastName,
       (STRFTIME('%Y', 'now') - STRFTIME('%Y', HireDate)) AS Years
FROM Employees
WHERE Years >= 15
ORDER BY LastName ASC
    
# Q4
Profiling the Customers table, answer the following question.
Are there any columns with null values?
# V1 
SELECT FirstName, PostalCode, Address, Phone,Fax, Company
FROM Customers
WHERE (PostalCode)is NULL

# V2 
SELECT COUNT(*)
FROM Customers
WHERE [column name] IS NULL   

# Q5 
Find the cities with the most customers and rank in descending order.
# V1 
SELECT City, 
    Count(Customerid) AS Total
FROM Customers 
WHERE Total = 2
# V2 
SELECT City, 
    Count(Customerid) AS Total
FROM Customers 
GROUP BY City
ORDER BY Total DESC;
    
# Q6 
Create a new customer invoice id by combining a customer’s invoice id with their first and last name
while ordering your query in the following order: firstname, lastname, and invoiceID.
    
# V1 
SELECT c.FirstName, c.LastName, i.InvoiceId,
    (c.FirstName||c.LastName||i.InvoiceId) AS NewInvoiceID
From Customers c 
LEFT JOIN Invoices i 
ON c.customerid = i.customerid
WHERE c.FirstName = "Astrid"
 
# V2 
ELECT i.InvoiceId,
       c.FirstName || c.LastName || i.InvoiceID AS id
FROM Customers c INNER JOIN Invoices i
ON c.CustomerId = i.CustomerID
WHERE id LIKE 'AstridGruber%'
    
    
    
    
    ### SQL WEEK 3Coding Assignment###
    
https://ucde-rey.s3.amazonaws.com/DSV1015/ChinookDatabaseSchema.png 
  
Q2 
#wrong answer: 

SELECT Firstname, lastname, email, city, customerid,(select count(*) as total
from invoices 
where invoices.customerid = customers.customerid) As total
from customers
group by customerid

#correct:
SELECT SUM(i.InvoiceID) AS totalnumber, c.FirstName, c.LastName, c.Email
From Invoices i INNER JOIN Customers c
ON i.CustomerID = c.CustomerID
GROUP BY c.CustomerID

## Q3  
# V1 
SELECT a.title, a.artistid, a.artistid,t.name,t.trackid
FROM albums a INNER JOIN tracks t 
ON a.albumid = t.albumid

##  ?? Q4 Retrieve a list with the managers last name, and the last name of the employees who report to him or her.

# V1
SELECT reportsto, employeeid, lastname
FROM employees 
? self join 

## Q5 Find the name and ID of the artists who do not have albums.

# V1
SELECT Artists.Name, Artists.Artistid 
FROM Artists 
LEFT JOIN Albums 
ON Artists.Artistid = Albums.Artistid
WHERE Albums.Artistid is NULL

# V2 
SELECT Name,
a.ArtistId,
b.Title
FROM Artists a
LEFT JOIN Albums b
ON a.ArtistId = b.ArtistId
WHERE b.Title IS NULL

## Q6 Use a UNION to create a list of all the employee's and customer's first names and last names ordered by the last name in descending order.

# V1 
SELECT lastname, firstname 
FROM Employees 
UNION 
SELECT lastname, firstname 
FROM customers 
ORDER BY lastname DESC;


## Q7 See if there are any customers who have a different city listed in their billing city versus their customer city.

SELECT c.city, i.billingcity
FROM customers c 
LEFT JOIN invoices i 
ON c.customerid!= i.customerid
WHERE c.customerid != i.customerid

  
  ### SQL WEEK 3 Practice Coding Quiz ###

## 1. How many albums does the artist Led Zeppelin have? 

SELECT art.Name
	,count(albumid) AS number
FROM artists art
LEFT JOIN albums al ON art.Artistid = al.Artistid
WHERE art.Name = "Led Zeppelin"

## 2. Create a list of album titles and the unit prices for the artist "Audioslave".

# a: 
SELECT ar.name, al.Title, al.albumid
From albums al
Left Join artists ar On ar.artistid = al.artistid
where ar.name ='Audioslave'

# getting info from three tables ? subqueries ? double joins? 

# b(Using a middle table as a bridge to get the info, inner join 2 times ) 
SELECT ar.name, al.Title, al.albumid
FROM albums al 
INNER JOIN tracks tr ON al.albumid = tr.albumid
INNER JOIN artists ar On ar.artistid = al.artistid
where ar.name ='Audioslave'

## 3. Find the first and last name of any customer who does not have an invoice. Are there any customers returned from the query?  
SELECT C.firstname, C.lastname, I.invoiceid
FROM customers C
LEFT JOIN invoices I  ON C.customerid = I.customerid
WHERE I.invoiceid IS NULL

## 4. Find the total price for each album.
# a.nested subqueries ? SUM functions ? 
SELECT  A.title, SUM(t.unitprice) AS total
FROM albums A 
INNER JOIN tracks T ON A.albumid = t.albumid
# b. Using "Group by"
SELECT A.Title, SUM(T.UnitPrice) AS total
FROM albums A
	INNER JOIN tracks T ON A.Albumid = T.Albumid
WHERE A.Title = 'Big Ones'
GROUP BY A.Title


## 5. How many records are created when you apply a Cartesian join to the invoice and invoice items table?
SELECT T.invoiceid
FROM invoice_items T
    CROSS JOIN Invoices I
    
