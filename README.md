# -BOOKSTORE-DBMS-PROJECT
# IST 659 PROJECT 

                               
                                           
                                  
                                            
                                                                                                         

  
                                               	
                                                                
                                                  
                                                       Project summary
The objective of the project is to build an automated database in the place of current manual entry system to better keep track of its customers, suppliers, order and stock. As bookstore has huge unorganized data and new data adding up every single day it is becoming hard for bookstore management to manage its customers data and keep a track of its supplies, order and inventory.
At present, the book store management system uses MS Excel, all records related to Sales, stock, Orders, Payment and Suppliers are stored in excel files. There is lot of duplicate and recurring data. When the records are changed they need to update each and every excel file. In case of Customer records, all information related to customers and the product which the customer has purchased is to be stored in the Customers excel files. Any changes or updates related to customer like his phone number, address, email etc. are manually updated in the excel.
To manage the whole data, the person maintaining records has to take great pain. Various excel files has to be maintained for each separate process. There is no option to find previous saved records. There is no security; anybody can access any report and sensitive data.
This system is designed to overcome all the above discussed problems of bookstore. This system is designed to record the customer’s purchase history, supplier’s incoming stock, Order history, current stock of books in the bookstore and all other transaction details between customer- bookstore and bookstore-supplier. Thus, this could be an inventory system which helps to keep a track of what you have in your store, informs you when you need to purchase more stock or supplies by category and maintains all customers data which can be used for analyzing the buying trends of customers. However, this project design is limited to maintain customer data base only and not used for analyzing customer buying trends.

This data base management system is designed to overcome data redundancy issues, automate data management and reduce manual effort. This system is designed to easily store, manage, update and retrieve data when needed. This report comprises of proposed bookstore                                                                                                                 



Business Rules:

•	Every customer must have at least one order associated with the bookstore.
•	Every order must have at least one item.
•	Every item must be part of at least one order.
•	Every item is stored in one and only one shelf and every shelf contains only one particular item.
•	Shelf can have zero or more items.
•	Every boeok is printed by one and only one publisher and every publisher must publish at least one book.
•	Ever book is authored by one and only one author and every author must at least write one book.



                                 SQL SCRIPTS FOR CREATING AND INSERTING SAMPLE DATA:

Creating Tables:

CREATE TABLE BSCustomer

(

CustomerId VARCHAR(6) PRIMARY KEY NOT NULL,

Name CHAR(10) NOT NULL,

DOB DATE NOT NULL,

Address CHAR(15) NOT NULL,

city CHAR(5)  NOT NULL,

State CHAR(5) NOT NULL,

);

 
Create Table Publisher
(
PublisherId VARCHAR(6) PRIMARY KEY NOT NULL,
Name VARCHAR(15) NOT NULL,
FullAddress VARCHAR(20) NOT NULL
);


Create Table Author
(
AuthorId VARCHAR(6) PRIMARY KEY NOT NULL,
FullName VARCHAR(15) NOT NULL,
Country VARCHAR(20) NOT NULL
);


CREATE TABLE BOOK
(

BookName VARCHAR(30) PRIMARY KEY NOT NULL,
ISBN VARCHAR(13)  NOT NULL,
AuthorId VARCHAR(6) NOT NULL,
PublisherId VARCHAR(6) NOT NULL,
CONSTRAINT BOOK_FK_AUTHID FOREIGN KEY(AuthorId) REFERENCES Author(AuthorId),
CONSTRAINT BOOK_FK_PUBID FOREIGN KEY(PublisherId) REFERENCES Publisher(PublisherId)

);

DROP TABLE BOOK

CREATE TABLE Shelf

(

ShelfNumber VARCHAR(6) PRIMARY KEY NOT NULL,

BookName VARCHAR(30) NOT NULL,

QuantityAvailable INTEGER NOT NULL,

CONSTRAINT Shelf_FK FOREIGN KEY(BookName) REFERENCES BOOK(BookName)

);


CREATE TABLE BSProduct

(
ProductId VARCHAR(6) PRIMARY KEY NOT NULL,
ProductName VARCHAR(30) NOT NULL UNIQUE,
ShelfNumber VARCHAR(6) NOT NULL,
ProductPrice Decimal(4,2) NOT NULL,

constraint BSProduct_FK FOREIGN KEY(ShelfNumber) REFERENCES Shelf(ShelfNumber)
);


CREATE TABLE CUSORDER

(
OrderId VARCHAR(6) PRIMARY KEY NOT NULL,
OrderDate DATETIME DEFAULT getdate(),
CustomerId VARCHAR(6) NOT NULL,
Qunatity INT NOT NULL,
TotalBillingAmount DECIMAL(6,2) NOT NULL,


constraint order_FK FOREIGN KEY(CustomerId) REFERENCES BSCustomer(CustomerId)

);

CREATE TABLE BSOrderLine
(
ProductId VARCHAR(6) NOT NULL,
OrderId VARCHAR(6) NOT NULL,
CONSTRAINT BSOrderLine_PK PRIMARY KEY(ProductId,OrderId),
CONSTRAINT BSOrderLine_FK_ProductId FOREIGN KEY(ProductId) REFERENCES BSProduct(ProductId),
CONSTRAINT BSOrderLine_FK_OrderId FOREIGN KEY(OrderId) REFERENCES CUSORDER(OrderId)

);


Inserting Data:


INSERT INTO BSCustomer VALUES( '123546', 'srinivas', '5/6/1994', 'Lexigton','syra','Ny');
INSERT INTO BSCustomer VALUES( '123547', 'Ben', '7/6/1984', 'Westcott','syra','Ny');
INSERT INTO BSCustomer VALUES( '123548', 'ron', '8/6/1984', 'cherry','syra','Ny');
INSERT INTO BSCustomer VALUES( '123549', 'tim', '9/6/1984', 'southbeach','syra','Ny');
INSERT INTO BSCustomer VALUES( '123550', 'kim', '1/6/1984', 'dell','syra','Ny');
INSERT INTO BSCustomer VALUES( '123551', 'chan', '6/6/1984', 'eastgen','syra','Ny');
INSERT INTO BSCustomer VALUES( '123552', 'wang', '5/6/1984', 'columbus','syra','Ny');



INSERT INTO Publisher VALUES( '657675', 'mchill', 'texas');
INSERT INTO Publisher VALUES( '657676', 'pearson', 'hyderabad');
INSERT INTO Publisher VALUES( '657677', 'arihant', 'syracuse');
INSERT INTO Publisher VALUES( '657678', 'tata', 'warangal');
INSERT INTO Publisher VALUES( '657679', 'cingage', 'hanamkonda');

select *from Publisher

INSERT INTO Author VALUES( '43523', 'reddy', 'India');
INSERT INTO Author VALUES( '93524', 'jim', 'UK');
INSERT INTO Author VALUES( '53943', 'RAM', 'India');
INSERT INTO Author VALUES( '12223', 'SHIVA', 'India');
INSERT INTO Author VALUES( '93523', 'DIN', 'USA');


INSERT INTO CUSORDER VALUES  ( '275432', '11/27/2018', '123546','5', '50');
INSERT INTO CUSORDER VALUES  ( '675632', '10/27/2018', '123547','1', '20');
INSERT INTO CUSORDER VALUES  ( '976132', '11/27/2018', '123548','9', '100');

INSERT INTO BOOK VALUES  ( 'intro to ds','123456789', '93523','657679');
INSERT INTO BOOK VALUES  ( 'modern DBMS', '122237658','43523','657678');
INSERT INTO BOOK VALUES  ( 'data analytics', '53943987651','12223','657676');

select* from BSProduct
INSERT INTO shelf VALUES( '453', 'intro to ds', '10');
INSERT INTO shelf VALUES( '67', 'modern DBMS', '12');
INSERT INTO shelf VALUES( '912', 'data analytics', '18');

INSERT INTO BSProduct VALUES  ( '11111', 'intro to ds', '453', '50');
INSERT INTO BSProduct VALUES  ( '11112', 'modern DBMS', '67', '39');
INSERT INTO BSProduct VALUES  ( '11113', 'data analytics', '912', '21');

INSERT INTO BSOrderLine VALUES  ( '11111', '275432');
INSERT INTO BSOrderLine VALUES  ( '11112', '675632');
INSERT INTO BSOrderLine VALUES  ( '11113', '976132');

1)	Sales summary report
SQL query

Data question : 1)	Generating Daily sales Report

SELECT DISTINCTROW Format$([dbo_CUSORDER].[OrderDate],'Long Date') AS [OrderDate By Day], Sum(dbo_CUSORDER.TotalBillingAmount) AS [Sum Of TotalBillingAmount]
FROM dbo_CUSORDER
GROUP BY Format$([dbo_CUSORDER].[OrderDate],'Long Date');


2)	Customer Purchase History Report
Sql Query



SELECT DISTINCTROW dbo_CUSORDER.OrderId, Format$([dbo_CUSORDER].[OrderDate],'Long Date') AS [OrderDate By Day], dbo_CUSORDER.CustomerId, dbo_BSCustomer.Name, dbo_BSCustomer.State, Sum(dbo_CUSORDER.TotalBillingAmount) AS [Sum Of TotalBillingAmount]
FROM dbo_BSCustomer INNER JOIN dbo_CUSORDER ON dbo_BSCustomer.[CustomerId] = dbo_CUSORDER.[CustomerId]
GROUP BY dbo_CUSORDER.OrderId, Format$([dbo_CUSORDER].[OrderDate],'Long Date'), dbo_CUSORDER.CustomerId, dbo_BSCustomer.Name, dbo_BSCustomer.State;



3)	Book and its availability
Sql query


SELECT dbo_Author.AuthorId, dbo_Author.FullName, dbo_BOOK.BookName, dbo_BOOK.ISBN, dbo_Shelf.ShelfNumber, dbo_Shelf.QuantityAvailable
FROM (dbo_Author INNER JOIN dbo_BOOK ON dbo_Author.[AuthorId] = dbo_BOOK.[AuthorId]) INNER JOIN dbo_Shelf ON dbo_BOOK.[BookName] = dbo_Shelf.[BookName];


