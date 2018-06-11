# SQL and Databases

-
-
# Relational databases

- Relational databases store data as a set of **related** fields (A "relation")
- Multiple relations across the same fields are stored together in a Table
- Relations -> Rows
- Fields -> Columns
- Collection of relations/rows -> Table

-
#### One way to think of it

- Object ~= Row (In the beginning anyway)
- Class ~= Table (again, in the beginning -- arrays make this tricky)
- Fields/Properties ~= Columns

-
### SQL

- Structured Query Language
- A standardized way to communicate with databases
- Heavily used on most relational databases
- Many different dialects with small variations 

-
### Non-relational, schemaless, and noSQL databases:

- Slightly different meanings, often used to just mean "Not SQL"
- Take different approaches to data storage and retrieval
- Often borrow some principles from SQL/Relational databases
- Beyond the scope of this lesson

-
### Our Environment:

- H2 embedded database
- Oracle dialect
- running in Spring Boot
- Using Hibernate ORM (more on that later)

-
-
### SQL Commands

-
-
### CREATE TABLE
- Creates a table.
- Must be done before putting any data in.
- This is where you define what your table will look like.
- Can be modified later by doing an ALTER TABLE.
- Defines the table name, column names, datatypes, and special rules / constraints.
- Also how you link to other tables.

-
### CREATE TABLE syntax
```SQL
CREATE TABLE table_name (
  column1 datatype,
  column2 datatype,
  column3 datatype
);
```

-
### CREATE TABLE example
```SQL
CREATE TABLE cars (
  id INT NOT NULL AUTO_INCREMENT,
  make VARCHAR(255) NOT NULL,
  model VARCHAR(255) NOT NULL,
  year VARCHAR(4) NOT NULL DEFAULT '2017',
  mileage INT(6),
  PRIMARY KEY (id),
  CONSTRAINT unique_make_model_year UNIQUE (make, model, year)
);

CREATE TABLE auto_prices (
  id INT PRIMARY KEY AUTO_INCREMENT,
  car_id INT REFERENCES cars(id),
  package VARCHAR(15) NOT NULL,
  price NUMBER(10,2) NOT NULL CHECK(price > 0),
  CONSTRAINT unique_package_per_car UNIQUE (car_id, package)
);
```

-
-

### INSERT
- Puts data into a table.
- Must provide all required values.
  - This means columns that specify the following:
    - `NOT NULL`
    - Do not have a `DEFAULT`
    - Are not `AUTO_INCREMENT` columns
- If `NULL` values are allowed, those columns can be excluded in the `INSERT`.

-
### INSERT syntax -- No column names
- Without specifying columns, you must make sure that every column is accounted for in the insert.
```SQL
INSERT INTO table
VALUES(column1_value, column2_value);
```

-
### INSERT example -- No column names
```SQL
INSERT INTO cars
VALUES(1, 'Honda', 'Civic', '2001', '98765');
```

-
### INSERT syntax -- With column names
```SQL
INSERT INTO table(column1_name, column2_name)
VALUES(column1_value, column2_value);
```

-
### INSERT example -- With column names
```SQL
INSERT INTO cars(make, model)
VALUES('Mazda', '3'),
('Volkswagen', 'Jetta'),
('Honda', 'Civic');

INSERT INTO auto_prices(car_id, package, price)
VALUES(1, 'EX', 2987.99);
```

-
### INSERT example -- With column names
Notice, we didn't use `id`, `year`, or `mileage` in our examples.  This works because:

- `id` has `AUTO_INCREMENT`
- `year` has a `DEFAULT` of `'2017'`
- `mileage` can be null

-
The table now contains:

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|1|Honda|Civic|2001|98765|
|2|Mazda|3|2017|null|
|3|Volkswagen|Jetta|2017|null|
|4|Honda|Civic|2017|null|

-
### INSERT constraints
Since `make` and `model` have the `NOT NULL` constraint, and we have the `unique_make_model_year` constraint
defined on the table, we must obey them.  Therefore, we cannot do the following:

-
### INSERT constraints
```SQL
INSERT INTO cars(make)
VALUES('Ford');
```
This gives us the following error:

`NULL not allowed for column "MODEL"`

-
### INSERT constraints
```SQL
INSERT INTO cars(make, model)
VALUES('Mazda', '3');
```
This gives us:

`Unique index or primary key violation: "UNIQUE_MAKE_MODEL_YEAR_INDEX_1 ON PUBLIC.CARS(MAKE, MODEL, YEAR) VALUES ('Mazda', '3', '2017', 2)"`

-
-
#### SELECT/FROM

- How you 'get' the data from the database.
- Specify desired columns, or get all columns with `*`
- Used with aggregate functions like `COUNT()` and `SUM()`

-
### SELECT syntax
```SQL
SELECT * FROM table;
```
or
```SQL
SELECT column1, column2
FROM table;
```

-
### SELECT example
```SQL
SELECT * FROM cars;
```
returns

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|1|Honda|Civic|2001|98765|
|2|Mazda|3|2017|null|
|3|Volkswagen|Jetta|2017|null|
|4|Honda|Civic|2017|null|

-
### SELECT example
```SQL
SELECT make, model FROM car;
```
returns

|MAKE|MODEL|
|-|-|
|Honda|Civic|
|Honda|Civic|
|Mazda|3|
|Volkswagen|Jetta|

-
### SELECT with functions
We can use built in SQL functions in our select statements like:
- MIN()
- MAX()
- COUNT()
- AVG()
- SUM()

-
### SELECT with functions syntax
```SQL
SELECT FUNC(column_name)
FROM table;
```

-
### SELECT with functions example
```SQL
SELECT MIN(year)
FROM cars;
```
returns

|MAX(YEAR)|
|-|
|2017|

-
-
#### WHERE
- Helps to refine queries/filter out results
- Uses operators for the filtering
- Refine further with `AND` or `OR` clauses

-
### WHERE operators

|Symbol|Purpose|
|-|-|
|=|Equal|
|<>|Not equal|
|>|Greater than|
|<|Less than|
|>=|Greater than or equal|
|<=|Less than or equal|
|BETWEEN|Between an inclusive range|
|LIKE|Search for a pattern|
|IN|To specify multiple possible values for a column|

-
### WHERE syntax
```SQL
SELECT * FROM table
WHERE column_name {operator} bound
```

-
### WHERE example
```SQL
SELECT * FROM cars
WHERE make = 'Mazda';

SELECT make, model FROM cars
WHERE year < 2017;

SELECT COUNT(*) FROM cars
WHERE year > 2010;
```

-
### WHERE with AND / OR syntax
```SQL
SELECT * FROM table
WHERE column_name {operator} bound
{AND / OR} column_name {operator} bound
```

-
### WHERE with AND / OR example
```SQL
SELECT * FROM cars
WHERE make = 'Mazda'
OR make = 'Volkswagen';

SELECT * FROM CARS
WHERE model = 'Civic'
AND year = '2017';
```

-
-
#### ORDER BY

- Orders the results
- Specify the column to sort on
- Can use multiple arguments to break ties
- Optional Ascending (`ASC`) or Descending (`DESC`) operator
  - `ASC` by default

-
### ORDER BY syntax
```SQL
SELECT column1, column2
FROM table
ORDER BY column1 {ASC / DESC}
```

-
### ORDER BY example
```SQL
SELECT * FROM cars;

SELECT * FROM cars
ORDER BY model;

SELECT * FROM cars
ORDER BY model DESC;

SELECT * FROM cars
ORDER BY make, year;

SELECT * FROM cars
ORDER BY make DESC, year DESC;

SELECT make, year FROM cars
WHERE make IN ('Mazda', 'Honda')
ORDER BY model DESC;
```

-
-
#### GROUP BY

- Group results together by a column value
- Allows category-specific aggregate calculations
- Must collapse the groups into the SELECTed values

-
### GROUP BY syntax
```SQL
SELECT FUNC(column) --Standard SELECT stuff
FROM table
GROUP BY column
```

-
### GROUP BY example
```SQL
SELECT make, model, COUNT(*)
FROM cars
GROUP BY make;
```
returns

|MAKE|MODEL|COUNT(*)|
|-|-|-|
|Honda|Civic|2|
|Mazda|3|1|
|Volkswagen|Jetta|1|

-
### GROUP BY example (broken)
```SQL
SELECT make, model, year, COUNT(*)
FROM cars
GROUP BY make;
```
returns the error

`Column "YEAR" must be in the GROUP BY list`

because the Hondas have different, so it can't be just grouped by make.

-
-
#### UPDATE
- Change existing values in database
- Affects all values in a column unless `WHERE` is present

-
### UPDATE syntax
```SQL
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

-
### UPDATE example
```SQL
UPDATE cars
SET year = '2018'
WHERE make = 'Honda'
AND year = '2017';
```
The table now contains

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|1|Honda|Civic|2001|98765|
|2|Mazda|3|2017|null|
|3|Volkswagen|Jetta|2017|null|
|4|Honda|Civic|2018|null|

-
### UPDATE without WHERE example
```SQL
UPDATE cars
SET mileage = 0;
```
The table now contains

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|1|Honda|Civic|2001|0|
|2|Mazda|3|2017|0|
|3|Volkswagen|Jetta|2017|0|
|4|Honda|Civic|2018|0|

-
-
#### DELETE
- Removes all selected rows
- Doesn't care if you didn't mean to hit `Enter` (No warning)
- BE CAREFUL!

-
### DELETE syntax
```SQL
DELETE FROM table
WHERE condition;
```

-
### DELETE example
```SQL
DELETE FROM cars
WHERE make = 'Honda'
AND model = 'Civic'
AND year = 2018;
```

-
### DELETE bad example
```SQL
DELETE FROM cars;
```
THIS WILL DELETE EVERYTHING IN THE TABLE!!!

-
-
#### Aliases (AS)
- Provides an alias for the named table or value
- Use it to make queries easier to read/shorter
- The AS is optional.

-
### Aliases syntax
```SQL
SELECT column_name AS alias
FROM table;
```

```SQL
SELECT column_name
FROM table AS t
```

```SQL
SELECT column_name AS column_alias
FROM table t;
```
-
### Aliases example
```SQL
SELECT c.make AS make, c.model AS model, c.year AS year, 
c.mileage miles, p.package package, p.price dollarydoos
FROM cars AS c, auto_prices p
WHERE c.id = p.car_id;
```
returns

|MAKE|MODEL|YEAR|MILES|PACKAGE|DOLLARYDOOS|
|-|-|-|-|-|-|
|Honda|Civic|2001|0|EX|2987.99|


-
-
### (Slightly)More advanced SQL
Everything that we just went through is enough to get you started with SQL.
This next part is touching on some more complex operations.

Realize that SQL stuff can get super complex, especially when it comes to query optimization
and overall DB design.  For now, we'll give you some more tools to be a little better, but
we've barely scratched the surface of SQL.

-
-
### LIKE and wildcards
SQL lets you use a `LIKE` operator in `WHERE` clauses with wildcards.

-
### LIKE and wildcards syntax
```SQL
SELECT * FROM table
WHERE column LIKE '%some%wildcard%matcher'
```

-
### LIKE and wildcards example
```SQL
SELECT * FROM cars
WHERE make LIKE '%a'
```
returns the following (because their `make`s all end in `a`)

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|1|Honda|Civic|2001|98765|
|2|Mazda|3|2017|null|

-
-
### HAVING 
`HAVING` essentially lets you have aggregate functions in your `WHERE` clause.

It must be used in conjunction with a GROUP BY clause.

-
### HAVING syntax
```SQL
SELECT * 
FROM table
GROUP BY column
HAVING COUNT(*) condition;
```

-
### HAVING example
Consider we have the following `cars` table:

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|8|Ford|Focus|2017|null|
|7|Honda|CRV|2017|null|
|1|Honda|Civic|2001|0|
|2|Mazda|3|2017|0|
|5|Mazda|6|2017|null|
|6|Volkswagen|GTI|2017|null|
|3|Volkswagen|Jetta|2017|0|

-
### HAVING example
```SQL
SELECT make
FROM cars
GROUP BY make
HAVING COUNT(make) > 1;
```
This will return just `Honda`, `Mazda`, and `Volkswagen` since `Ford` only has 1 model in our DB.

-
-
### UNION
Combines two or more `SELECT` statements.

This is used, typically, to take data from different tables that are of the same "type" and aggregate them.

-
### UNION syntax
```SQL
SELECT column_name FROM table1
UNION
SELECT other_column_name FROM table2;
```
-

### UNION example
Let's say we have another table like `cars`, but this one is `bikes` and we want to get all of the makes for every vehicle.

-
### UNION example
```SQL
SELECT make FROM cars
UNION
SELECT make FROM bikes;
```
would yield something like:

|MAKE|
|-|
|Honda|
|Mazda|
|Ford|
|Volkswagen|
|Triumph|
|Harley-Davidson|

-
-
### JOIN
A `JOIN` combines rows from multiple tables on a column that they "share".

`JOINS` are super important in SQL.  They're where the heavy lifting of SQL pays off, as well as a place where
you can make a lot of mistakes and have terrible performance.

-
There are multiple types of JOIN.  The main ones are:
- INNTER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN

We're not going to get into all of that now, though.  We'll just do one quick JOIN example.

-
### JOIN syntax
```SQL
SELECT column1, column2
FROM table1
JOIN table2 ON table1.column1 = table2.columnN;
```

-
### JOIN example
Remember how we made both `cars` and `auto_prices` tables.  We'll let's make sure they're both populated pretty well.

-
### JOIN example
Consider we have the following `cars` table:

|ID|MAKE|MODEL|YEAR|MILEAGE|
|-|-|-|-|-|
|1|Honda|Civic|2001|98765|
|2|Mazda|3|2017|null|
|3|Volkswagen|Jetta|2017|null|
|4|Honda|Civic|2017|null|

-
### JOIN example
Consider we have the following for the `auto_prices` table:

|ID|CAR_ID|PACKAGE|PRICE|
|-|-|-|-|
|1|1|EX|2987.99|
|2|2|Sport|18095|
|3|2|Touring|21140|
|4|2|GrandTouring|24195|
|5|3|S|17895|
|6|3|SE|20895|
|7|3|SEL|24995|
|8|4|LX|18740|
|9|4|EX|21140|

-
### JOIN example
What we actually want is the make, model, package, and price for every car.  We can do this with a join.

```SQL
SELECT c.make, c.model, p.package, p.price
FROM cars c
JOIN auto_prices p ON c.id = p.car_id;
```
returns

-
|MAKE|MODEL|PACKAGE|PRICE|
|-|-|-|-|
|Honda|Civic|EX|2987.99|
|Mazda|3|Sport|18095|
|Mazda|3|Touring|21140|
|Mazda|3|GrandTouring|24195|
|Volkswagen|Jetta|S|17895|
|Volkswagen|Jetta|SE|20895|
|Volkswagen|Jetta|SEL|24995|
|Honda|Civic|LX|18740|
|Honda|Civic|EX|21140|

-
-
### Resources

- [SQL Zoo](http://sqlzoo.net/)
- [H2 Reference](http://www.h2database.com/html/main.html)
- Code School [Try SQL](https://www.codeschool.com/courses/try-sql)
- W3 School[SQL References](https://www.w3schools.com/sql/)