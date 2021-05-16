# SQL-for-Data-Science
UC Davis's SQL for Data Science course notes

# Creating Tables
- Specifiying if the field is nullable or not is required.
- A primary key MUST exist. It can't not be null.
```
CREATE TABLE table_name 
(
Id int(10) PRIMARY KEY,
name string NOT NULL,
description varchar(100) NULL
)
```
## Temporary Tables
- They work exactly the same as normal tables. The information will be only avaliable until the SQL sesion is open.
- They are useful when a more complex query is required. 

# Selecting information
- Multiple relational operators are avaliable  >, <, >=, !

The sintaxis is simple. 
```
SELECT <columnas>
FROM <tabla>
LIMIT <numero de resultados>
```
## Limiting (not filtering) results

The sintaxis is simple. 
```
SELECT <columnas>
FROM <tabla>
LIMIT <numero de resultados>
```
## Advanced Filtering: IN, OR, and NOT
### Between Operator
It requieres a range, it selects multiple registers that match this range.
```
SELECT * FROM Shoes WHERE value BETWEEN 100 AND 200 LIMIT (30);
```
This example will SELECT the first 30 registers (if avaliable) that have a value BETWEEN 100 AND 200 $. 

### In Operator
- It is use for selecting specifyc information (for example, IDS), it doesnt work with ranges.
- The values must be separed with comas.
- Faster than OR.
- Allows more values than OR
```
SELECT * FROM Shoes WHERE Id IN (3,5,1,10)
```
This example will select all the registers with Id: 3,5,1,10. 

### Or Operator
The second condition will be not evaluate if the first condition its true.
Only two results. 
```
SELECT * FROM Shoes WHERE Brand = 'Adidas' OR 'Nike'
```
If there are any shoes with the Adidas's brand, the shoes with Nike's brand will be search.

It can be mix with another operators
```
SELECT * FROM Shoes WHERE (SuplierId = 9 OR SuplierID = 11) AND Price > 15 ...
```
### NOT
Is like !, use it before the statemnent
```
SELECT * FROM Shoes WHERE NOT Brand 'Nike' AND NOT Brand 'Adidas';
```
## Wildcards
### Like Operator (Predicate, not operator :P)
- There are special characters used to match parts of a value.
- Only works for Strings.
```
%Pizza - Any String that ends with pizza will match
Pizza% - Any String that starts with pizza will match
%Pizza% Any String before or after pizza.
a%gmail.com Any String that starts with "a" and ends with gmail.com
```
## Sorting with ORDER BY
Allows to give a logical order to the data retrived. Normally it is random order (input order), the data can change or be deleted.
- It will order Alphabetical or ascending/descending depending on the data type. 
- Must be the last clause.
- Can sort multiple columns, just add a , between them.

```
SELECT Something FROM database WHERE ... ORDER BY field
```
- Also can sort by colum order

```
SELECT Something FROM database ORDER BY 3,6 
```
This will order by colum 3 and 6. 

### Sort direction
Only aplies to the column name it directly preceeds. 
- ASCending
- DESCending

## Math Operations
- This are avaliable + - * /

### Multiplication example
Units and Price are not required, but is a good practice to use it for checking everything is ok.
```
SELECT ID, Units, Price, Units*Price AS Total_amount FROM Orders 
```
## Aggregate Functions
- Pre-built functions.
- they are use to summarize data.
- Find Highest and lowest.
- Total number of rows.
- Average value.

### Count 

Count everything, including null values (will give the total rows in the table Orders
```
SELECT COUNT(*) AS Total FROM Orders
```
Count everything, IGNORING null values (will give the total rows in the table Orders
```
SELECT COUNT(OrderID) AS Total FROM Orders
```
- All the other operators work the same way. 
- The Distinc word helps to not operate duplicated rows. 

## Grouping Data with SQL
- Can contain multiple columns
- every column in thr SELECT statement must be present in a GROUP BY clase, except for aggetate functions.
- Nulls will be grouped together.
- HAVING is used to replace WHERE, it only works in the groups that GROUP BY will make. 
- It used with AGGREGATE FUNCTIONS

# SQL Joins
## Beefits of Breaking data into tables
- Efficient storage
- Easier manipulation
- Greater scalability
- Logically models a process


## Joins
![Captura de pantalla 2021-05-16 012849](https://user-images.githubusercontent.com/55632072/118387859-69f98d80-b5e6-11eb-9941-15e2c80dffea.png)

- Allows data retrieval freom multiple tables in one query. 
- Joins are not physical they persist for the duration of the query execution.


### Cartesian (Cross) Joins
- In this kind of join all the rows from the first table joins with all the rows of another table.
 For examen if we have the table A with 10 records, and the table B with 8 records, we will get a result of 80 records after executing the join.
 this is AxB
 - Not frequently used.
 - Computationally taxing

```
// Here we got the same typic select statment. 
Select product_name
,unit_price
,company_name
FROM suppliers
//then, we add the join statment
CROSS JOIN products;
```
### Inner Joins
- The INNER JOIN are use to slect records tat have matching values in both tables. 
- The first part (select statement) is the same. then just need to specify wich column is the common column in both tables.

```
// Here we got the same typic select statment. 
Select suppliers.company_name
,unit_price
,product_name
FROM suppliers
//then, we add the join statment
INNER JOIN products // specify the table we the new data will come from
ON suppliers.supplier_id = products.supplier_id;
```
There is not limit for joining multiples tables, but the performance of the query is goint to be affected with more than two teables.
I want to know the symptom observation and the symptom name, i am located at pivot table 'characterization_special_condition'
```
SELECT c.symptom_observation, s.name
FROM characterization_special_condition csc
INNER JOIN characterizations c ON csc.characterization_id = c.id
INNER JOIN special_conditions s ON csc.special_condition_id = s.id

```

### Aliases and Self Joins
SQL alisases just store for the duration of the query.
Match customers from the same city
```
SELECT A.CustomerName AS
CustomerName1
, B.CutomerName AS Customername2
, A.City
FROM Customers A, Customers B
WHERE A.customer_id = B.customer_id
AND A.city = B.city
ORDER BY A.city
```
### Left, Right and Full other Join
- SQL Lite only oes left Joins
- This works similar to inner join, in this case, ALL THE RECORDS from the left table will appear
- The result for rows that doesnt have the matching value, for example: customers without order_id will appear, and leave as null the information from the table at the right hand. 
- Without this, we will not be able to have customer that have not place an order (inner join).
- Right join does the same, but keeps the data from the table at the right hand, and leaves null matches that does not exist from table at the left hand.
- Finally outer joins, brings everything from both tables.

Please, bring all customers and it orders (if they have at least one)
```
SELECT c.name
, o.id
FROM Customers c
LEFT JOIN Orders o
ON c.order_id = o.id
ORDER BY c.name
```
Please, bring only the customers that have an order, and include the order id
```
SELECT c.name
,o.id
FROM Customers c
INNER JOIN Orders o
ON c.order_id = o.id
ORDER BY c.name
```
### Unions
- The union operator is user to combine the result-set of two or more SELECT statements
- Each SELECT statement within UNION must have the same number of columns.
- Columns must have similar data types
- The columns in each SELECT statement must be in the same order. 

```
SELECT column1
FROM table1
UNION
SELECT column2
FROM table2
```

# Adding registers.
There are two ways to add register to a SQL database, it can be the short way (this depends on the created order)
```
INSER INTO nombre_tabla 
VALUES
(
'1',
'Nombre 1',
'Descripcion 1',
);
```
This specifict way, specify the order of the columns and then the information to ve added
Do not use quotes for the column names, however, the information does require them.
```
INSER INTO nombre_tabla
(
Id,
name,
description
)
VALUES
(
'1',
'Nombre 1',
'Descripcion 1',
)
```
