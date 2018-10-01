# PostgreSQL Notes
#cs #databases

## DBMS
Database management system is a program used to access a database. it allows user to manipulate data, control access to a database
Relational DBMS defines database relationships in tables
(Microsoft SQL, MySQLm Oracle, and PostgreSQL)

SQL - structured query language - used for inserting, updating, deleting data from table

## The SELECT Command
`SELECT column_name FROM table_name;`
`SELECT column_name1, column_name2, column_name3 FROM table_name;`
`SELECT * FROM table_name;`

### Where Clause
`SELECT * from table_name WHERE column_name = value`
```
SELECT column_name FROM table_name
WHERE column_name1 = 'value1'
AND column_name2 = 'value2'
```


### IN statement

```
SELECT * FROM table_name 
WHERE column_name IN (‘Value1’, ‘Value2’);
```
```
SELECT * FROM table_name 
WHERE column_name NOT IN (‘Value1’, ‘Value2’);
```

### BETWEEN statement

`SELECT * FROM table_name WHERE column_name BETWEEN 5 AND 10;`
```
SELECT * FROM table_name 
WHERE date BETWEEN 'yyyymmdd' AND 'yyyymmdd';
```

### LIKE statement
`SELECT * FROM table_name WHERE column_name LIKE '%substring'`

‘%’ - any number of character on that side of the substring
‘_’ - one character on that side of the substring

### ORDER BY statement
can order by int, string, and date
`SELECT * FROM table_name ORDER BY column_name;`  Ascending
`SELECT * FROM table_name ORDER BY column_name ASC;`  Ascending
`SELECT * FROM table_name ORDER BY column_name DESC;`  Descending

### Limiting Results
`SELECT * FROM table_name ORDER BY column_name LIMIT 5;` 
You can OFFSET to “skip a number of entries”
`SELECT * FROM table_name ORDER BY column_name LIMIT 5 OFFSET 2;` 

### DISTINCT statment
`SELECT DISTINCT column_name from table_name;`

### Setting a column name alias
do not use alias name in WHERE clauses, you must use the original database column name.
`SELECT first_name, last_name AS surname, pay AS pay_per_hour FROM employees`


## Selecting Data from Multiple Tables
### JOIN statment
allows you to retrieve data from multiple tables in a single SELECT statement
To join two tables there needs to be a related column between them

### Inner JOIN
Will retrieve data only when there is matching values in both tables
```
SELECT customers.first_name, customers.last_name, orders.quantity, orders.price FROM orders
INNER JOIN customers ON orders.customer_id = customers.id;

SELECT c.first_name, c.last_name, o.quantity, o.price FROM orders o
JOIN customers c ON o.customer_id = c.id;
```

### Left JOIN
Will retrieve all data from the left table, as well as any matching rows from the right table
table 1: orders, table 2: customers.
Will pull all orders, and customers who’s id shows up as an order’s customer_id
```
SELECT c.first_name, c.last_name, o.quantity, o.price FROM orders o
LEFT JOIN customers c ON o.customer_id = c.id;
```

### Right Join 
Will retrieve all data from the right table, as well as any matching rows from the left table
```
SELECT c.first_name, c.last_name, o.quantity, o.price FROM orders o
Right JOIN customers c ON o.customer_id = c.id;
```

### Full Join
Will retrieve all data from both the left and right tables
```
SELECT c.first_name, c.last_name, o.quantity, o.price FROM orders o
FULL JOIN customers c ON o.customer_id = c.id;
```

## Join more than two tables
```
SELECT * FROM orders o 
JOIN products p ON o.product_id = p.id
JOIN customers c ON o.customer_id = c.id; 
```

## UNION and UNION ALL
Combines the results set of two SELECT statements
the SELECT statements must have the same number of columns
and the columns must be of compatible data types

UNION removes duplicates where as UNION ALL does not
```
SELECT first_name, last_name FROM customers
UNION
SELECT first_name, last_name FROM employees;
```
```
SELECT first_name, last_name FROM customers
UNION ALL
SELECT first_name, last_name FROM employees;
```

## Aggregate Functions
## Count Function
returns the number of rows within a SELECT statement
```
SELECT COUNT(*) FROM products
WHERE season = 'Summer';
```

## Min and Max Functions
`SELECT MIN(wholesale_price) AS cheapest_product FROM products;`

## Sum Function
`SELECT SUM(hours) FROM employees;`
```
SELECT SUM(quantity) FROM orders
WHERE order_date BETWEEN '20170101' AND '20170131';
```

## Average Function
`SELECT AVG(pay) FROM employees`

## Group By Clause
this will sum the total price of all orders for each unique customer_id
```
SELECT customer_id, SUM(price) FROM orders
GROUP BY customer_id;
```

## Having Clause
```
SELECT customer_id, SUM(price) FROM orders
GROUP BY customer_id
HAVING customer_id = 8;
```

## Data Definition Language
* A subset of SQL which defines the structure of database objects such as tables, indexes, and views.
* Used to create, modify, and remove database objects in a database
* Not used for inputting, updating, or removing data from databases

## Data Types
`INT`: integer
`DECIMAL(4,2)`: decimal (first param is number of decimals before decimal point, second param is number of decimals after)
`NUMERIC`: Same as decimal
`SERIAL`: Automatically increments with every new row

`CHAR(n)`: Fixed Length string
`VARCHAR(n)`: Varying length string, n is the max length

`BOOLEAN`: Boolean contains either true or false values

`DATE`: date contains year, month, day.
`TIME`: time contains hour, minute, second.
`TIMESTAMP`: Timestamp contains year, month, day, hour, minute, seconds

## Foreign Keys
A foreign key is used to link two tables together
A foreign key is a column whose values match the values of another tables primary key column
The table with the primary key is called the reference, or parent, table and the table with the foreign key is called the child table
A table can have multiple foreign keys

## Creating tables
```
CREATE TABLE movies(
	id SERIAL PRIMARY KEY,
	name VARCHAR(100) UNIQUE NOT NULL,
	release_date DATE NOT NULL,
	director INT REFERENCES directors(id),
	lead_actor INT,
	rating INT CHECK (rating BETWEEN 1 AND 100)
);
```

## Modifying Tables
```
ALTER TABLE table_name
ADD column_name VARCHAR(20)

ALTER TABLE movies
DROP rating;

ALTER TABLE movies
ADD FOREIGN KEY (lead_actor) REFERENCES actors(id);
```

## Adding/Dropping Constraints
```
ALTER TABLE table_name 
ADD CONSTRAINT unique_name UNIQUE (name);

ALTER TABLE table_name DROP CONSTRAINT unique_name;

ALTER TABLE table_name ALTER COLUMN column_name SET NOT NULL;

ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL;
```

## Altering column data types
```
ALTER TABLE table_name 
ALTER COLUMN column_name TYPE VARCHAR(40);
```

## Removing a table
`DROP TABLE table_name;`

## Removing all rows from table (remove data)
`TRUNCATE TABLE table_name;`

## Data Manipulation Language
used to insert update, and remove data from tables within a database

## Insert Data from table
```
INSERT INTO directors(name, date_of_birth, awards, nationality)
VALUES ("Christopher Nolan", "19630120", 7, "American");
```

## Update Data from table
```
UPDATE directors SET nationality = 'British'
WHERE name = 'Danny Boyle';
```

## Delete Data from table
```
DELETE FROM directors WHERE id = 10;
```