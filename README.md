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
    
