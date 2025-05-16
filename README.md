📚 Bookstore SQL Data Analysis Project
This project simulates a bookstore database system using PostgreSQL. It includes table creation, data retrieval, and analytical SQL queries to explore customer behavior, sales trends, inventory insights, and book performance

``` sql
DROP TABLE IF EXISTS BOOKS;
CREATE TABLE BOOKS (
Books_ID SERIAL PRIMARY KEY,
Title VARCHAR (100),
Author VARCHAR(100),
Genre VARCHAR(50),
Published_Year INT,
Price NUMERIC(10,2),
Stock INT
);```



```sql
CREATE TABLE Customers (
Customer_ID SERIAL PRIMARY KEY,
Name VARCHAR(100),
Email VARCHAR(100),
Phone VARCHAR(15),
City VARCHAR(50),
Country VARCHAR(150)
);```



```sql
CREATE TABLE orders (
Order_ID SERIAL PRIMARY KEY,
Customer_ID INT REFERENCES Customers(Customer_ID),
Book_ID INT REFERENCES Books(Books_ID),
Order_Date Date,
Quantity INT,
Total_Amount Numeric(10,2)
);```

SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;



-- 1) Retrieve all books in the Fiction genre:

``` sql
SELECT * FROM BOOKS
WHERE GENRE = 'Fiction';```



-- 2) Find books published after the year 1950:
```sql
SELECT * FROM BOOKS
WHERE PUBLISHED_YEAR > 1950;```




-- 3) List all customers from the Canada
```sql
SELECT * FROM CUSTOMERS
WHERE COUNTRY = 'Canada';```



-- 4) Show orders placed in November 2023:
``` sql
SELECT * FROM ORDERS
WHERE ORDER_DATE BETWEEN '2023-11-01' AND '2023-11-30';```



-- 5) Retrieve the total stock of books available:
``` sql
SELECT SUM(STOCK) AS TOTAL_STOCK FROM BOOKS;```



-- 6) Find the details of the most expensive book:
``` sql
SELECT MAX(PRICE) AS MOST_EXPENSIVE FROM BOOKS;

SELECT * FROM BOOKS ORDER BY PRICE DESC LIMIT 1;```



-- 7) Show all customers who ordered more than 1 quantity of a book:
```sql
SELECT * FROM ORDERS
WHERE QUANTITY>1;```



-- 8) Retrieve all orders where the total amount exceeds $ 20;
```sql
SELECT ORDER_ID,TOTAL_AMOUNT FROM ORDERS
WHERE TOTAL_AMOUNT > 20;```



-- 9) List all genres available in the Books table:
```sql
SELECT DISTINCT GENRE FROM BOOKS;```



-- 10) Find the book with the lowest stock:
```sql
SELECT * FROM BOOKS
ORDER BY STOCK
LIMIT 10;```



-- 11) Calculate the total revenue generated from all orders:
```sql
SELECT SUM(TOTAL_AMOUNT) AS TOTAL_REVENUE 
FROM ORDERS;```



-- Advanced queries


-- 1) Retreive the total number of books sold for each genre
```sql
SELECT  GENRE, SUM(QUANTITY) AS TOTAL_QTY FROM ORDERS 
INNER JOIN BOOKS 
ON BOOK_ID = BOOKS_ID
GROUP BY GENRE;```



--2) Find the average price of books in the "Fantasy genre:
```sql SELECT AVG(PRICE) AS AVERAGE_PRICE FROM BOOKS
WHERE GENRE = 'Fantasy';```


 
-- 3) List customers who have placed at least 2 orders:
```sql SELECT A.CUSTOMER_ID, A.NAME, COUNT(B.ORDER_ID) AS TOTAL_ORDERS FROM CUSTOMERS AS A
INNER JOIN ORDERS AS B
ON A.CUSTOMER_ID = B.CUSTOMER_ID
GROUP BY A.CUSTOMER_ID, A.NAME
HAVING COUNT(B.ORDER_ID) >= 2; ```



-- 4) Find the most frequently ordered book
``` sql
SELECT A.BOOK_ID, B.TITLE, COUNT(A.ORDER_ID) AS ORDER_COUNT FROM ORDERS AS A
INNER JOIN BOOKS AS B
ON A.BOOK_ID = B.BOOKS_ID
GROUP BY A.BOOK_ID, B.TITLE
ORDER BY ORDER_COUNT DESC LIMIT 1;```




--5 Show the top 3 most expensive books of fanatasy genre
```sql
SELECT TITLE, PRICE FROM BOOKS
WHERE GENRE = 'Fantasy'
ORDER BY PRICE DESC LIMIT 5; ```



--6 Retrieve the total quantity of books sold by each author
``` sql
SELECT A.AUTHOR, SUM(B.QUANTITY) AS TOTAL_QTY FROM BOOKS AS A
INNER JOIN ORDERS AS B
ON A.BOOKS_ID = B.BOOK_ID
GROUP BY A.AUTHOR; ```



-- 7 List the cities where customers who spent over $30 are located
```sql
SELECT DISTINCT(B.CITY), A.TOTAL_AMOUNT FROM ORDERS AS A
INNER JOIN CUSTOMERS AS B
ON  A.CUSTOMER_ID = B.CUSTOMER_ID
WHERE A.TOTAL_AMOUNT > 30;```



-- 8) Find the customer who spent the most on orders:
```sql
SELECT C.NAME, SUM(O.TOTAL_AMOUNT) AS TOTAL_SPENT FROM CUSTOMERS AS C
INNER JOIN ORDERS AS O
ON  C.CUSTOMER_ID = O.CUSTOMER_ID
GROUP BY C.NAME
ORDER BY TOTAL_SPENT DESC LIMIT 1;```



--9) Calculate the stock remaining after fulfilling all orders:

```sql
SELECT B.BOOKS_ID, B.TITLE, B.STOCK, COALESCE(SUM(O.QUANTITY),0) AS ORDER_QUANTITY,
B.STOCK - COALESCE(SUM(O.QUANTITY),0) AS REMAINING_QUANTITY
FROM BOOKS AS B
LEFT JOIN ORDERS AS O
ON O.BOOK_ID = B.BOOKS_ID
GROUP BY B.BOOKS_ID;```
 





📦 Database Structure
* The project contains three core tables:

* BOOKS: Stores book details including title, author, genre, price, stock, and publication year.

* CUSTOMERS: Holds customer contact and location information.

* ORDERS: Tracks each order with quantity, total amount, and references to books and customers.

## Key Features
Table creation with proper primary and foreign keys

Basic SQL queries for data filtering and aggregation

Advanced SQL analytics using JOIN, GROUP BY, HAVING AND COALESCE

📈 Insights on:

Best-selling books and authors

Top-spending customers

Inventory tracking post-sales

Genre-wise and city-wise performance


## Tools & Technologies

- **Language**: SQL  
- **Database**: PostgreSQL



 
