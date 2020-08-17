# Introduction to JDBC

-
-


### What We'll Cover:

<ul>
<li class="fragment fade-up">What is the JDBC?</li>
<li class="fragment fade-up">Architecture of JDBC</li>
<li class="fragment fade-up">Role of Driver Manager</li>
<li class="fragment fade-up">Understanding JDBC Driver Types</li>
</ul>

-
-

## Objectives:
<ul>
<li class="fragment fade-up">Introduce JDBC</li>
<li class="fragment fade-up">Connect to SQL Database</li>
<li class="fragment fade-up">Perform CRUD operations</li>
</ul>
-

## JDBC: Introduction

JDBC is an API for the Java programming language that defines how a client may access a Database.

Part of Java SE, found in java.sql and javax.sql packages

-

![alt text](https://i.ytimg.com/vi/ni1PiUgKLDE/maxresdefault.jpg "JDBC API Relationship")

-

### Architecture of JDBC

* JDBC API - Application to JDBC Manager Connection
* JDBC Driver API - JDBC Manager to Driver Connection

-

### Driver Manager

The basic service for managing a set of jdbc drivers

-

![alt text](https://i.ytimg.com/vi/ni1PiUgKLDE/maxresdefault.jpg "JDBC API Relationship")

-

### JDBC Driver Types

A JDBC driver is a set of Java classes that implement the JDBC interfaces, targeting a specific Database

* The JDBC interfaces comes with standard Java
* Implementation of these interfaces is specific to the Database

-

__Types of Drivers__

* Type 1 : JDBC-ODBC bridge
* Type 2 : Native-API driver
* Type 3 : Network-Protocol driver (Middleware driver)
* Type 4 : Database-Protocol driver (Pure Java driver)

-

#### Type 1 : JDBC-ODBC bridge
__Advantages__

<ul>
<li class="fragment fade-up">It is very easy to use</li>
<li class="fragment fade-up">Almost any database is supported</li>
</ul>

__Disadvantages__
<ul>
<li class="fragment fade-up">Performance will not be efficient</li>
<li class="fragment fade-up">ODBC Driver needs to be installed</li>
<li class="fragment fade-up">Type 1 drivers are not portable</li>
<li class="fragment fade-up">Not suitable for Applets</li>
</ul>
-

#### Type 2 : Native-API driver
__Advantages__
<ul>
<li class="fragment fade-up">Faster than Type 1 Driver</li>
</ul>

__Disadvantages__
<ul>
<li class="fragment fade-up">Client Side library is not available for all databases</li>
<li class="fragment fade-up">Vendor Client Library needs to be installed</li>
<li class="fragment fade-up">It is a Platform Dependent
<li class="fragment fade-up">Not Thread Safe</a>
</ul>
-

#### Type 3 : Network-Protocol driver (Middleware driver)
__Advantages__
<ul>
<li class="fragment fade-up">No additional library installation is required on client system</li>
<li class="fragment fade-up">No changes are required at client for any DB</li>
<li class="fragment fade-up">Supports Caching of Connection, Query Results, Load Balancing Logging and Auditing etc.</li>
<li class="fragment fade-up">A Single Driver can handle any database provided the middleware supports it</li>
</ul>
__Disadvantages__
<ul>
<li class="fragment fade-up">Performance will be slow</li>
<li class="fragment fade-up">Requires Database-specific coding</li>
<li class="fragment fade-up">Maintenance of Network Protocol driver becomes costly</li>
</ul>
-

#### Type 4 : Database-Protocol driver (Pure Java driver)
__Advantages__
<ul>
<li class="fragment fade-up">Platform Independent</li>
<li class="fragment fade-up">No intermediate format is required</li>
<li class="fragment fade-up">Application connects directly to the database server</li>
<li class="fragment fade-up">Performance will be very fast</li>
<li class="fragment fade-up">JVM manages all aspects</li>
</ul>
__Disadvantages__

<li class="fragment fade-up">Drivers are database dependent</li>
</ul>
-

#### Overview : Recommendations:

* If you are accessing **one type** of database, such as Oracle, SQL Server, MYSQL etc., then the preferred driver is **Type 4**
* If your Java application is accessing **multiple types** of databases at the same time, **Type 3** is the preferred driver.
* **Type 2** drivers are useful in situations where a Type 3 or Type 4 driver is not available yet for your database
* The **Type 1** driver is not considered a deployment-level driver, and it is typically used for **development and testing purposes only**


-
-

## Connecting to a MYSQL Database

First, we must get the mysql connecter for java. This can be done using the pom.xml file.

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

-

The DriverManager object will need 3 pieces of information to create a connection to a database:

```
    static String username = "user name here";
    static String password = "user password here";
    static String dbUrl = "jdbc:mysql://localhost:3306/someDb";

```

-

Calling the _.getConnection()_ method with the previous variables as arguments will hopefully return a Connection object that references the connection to the database:

```
        Connection conn = null;

        try{
            conn = DriverManager.getConnection(dbUrl, username, password);

            System.out.println("Connection Established to MYSQL Database");

        } catch (SQLException e) {

            System.err.println(e.getMessage());

        } finally {

            conn.close();

        }

```

-

### Exception Handling

JDBC methods throw `SQLException`

`SQLException` extended from the `Exception` class

* getMessage()
* getLocalizedMessage()
* getErrorCode()
* getSQLState()
* getNextException()
* setNextException()
* iterator()

-

#### Summary

* Connecting to MYSQL Database using JDBC
* Managing Database resources
* Handling JDBC Exceptions

-
-

## CRUD using JDBC

* Executing Static SQL statements
* Understanding ResultSet
	* Scrollable ResultSet
	* Updateable ResultSet
* Understanding Prepared Statements
	* Retrieve (Select)
	* Insert (Create)
	* Update
	* Remove (Delete)

-

### Static SQL statement

Two Interfaces for executing static SQL statements:

* Statement
* ResultSet

-

#### Statement Interface

used for executing a static SQL Statement

__Methods__

* ResultSet executeQuery(String SQL)
* int executeUpdate(String SQL)
* boolean execute(String SQL) _*advanced usage only_

-

```Java
Connection conn DriverManager.getConnection(mySqlCS, mySqlUser, mySqlPwd);

Statement stmt = conn.createStatement();
```

-

### Steps for Development Process

1. Establish Connection to a Database
2. Create Statement Object
3. Execute SQL Query
4. Process the ResultSet

-

#### ResultSet Interface

A ResultSet is a multidimensional array that is returned by a database statement

java.sql.ResultSet interface represents the result set of a database query

-
#### ResultSet Interface

When the ResultSet is first returned, the starting cursor position is before the first row of data

-
#### ResultSet Interface

```
Connection conn DriverManager.getConnection(mySqlCS, mySqlUser, mySqlPwd);

Statement stmt = conn.createStatement();

ResultSet rs = stmt.executeQuery("select * from player");
```

-
#### ResultSet Interface Methods

Methods of the ResultSet can be divided into 3 categories

1. Navigational Methods
2. Get Methods
3. Update Methods

-

#### Navigational Methods

* beforeFirst()
* afterLast()
* first()
* last()
* absolute(n) _* n is a specific result index_
* relative(n) _* n is the direction to move from current index_
* previous()
* next()
* getRow()

-

#### Get Methods

* get method for each data type
	* .getInt(), .getString(), etc.
* Each get method has two versions
	1. One that takes a column name
	2. One that takes in a column index

```
rs.getString("first_name")
```

-

#### Scrollable ResultSets

__ResultSet Types__

* TYPE_ FORWARD_ONLY
* TYPE_ SCROLL_INSENSITIVE
* TYPE_ SCROLL_SENSITIVE

__ResultSet Concurrency Type__

* Concur_ Read_Only
* Concur_Updatable

-

#### Update Methods

Updatable ResultSet allows modification to data in a table through the ResultSet

* Each Update method has two versions
	1. One that takes in a column name
	2. One that takes in a column index

-

To update a String Column of the current row

* public void updateString(int columnIndex, String s)
* public void updateString(String columnName, String s)

_All update methods throw SQLException_

-

Making these changes only changes the data in the ResultSet Object not the underlying Database.

-

The following will make changes to the actual

* updateRow()
* deleteRow()
* refreshRow()
* cancelRowUpdates()
* insertRow()

-

### Prepared statements


<ul>
<li class="fragment fade-up">What is a`PreparedStatement`?</li>
<li class="fragment fade-up">How to create a `PreparedStatement`</li>
<li class="fragment fade-up">Setting Parameters of a `PreparedStatement`</li>
<li class="fragment fade-up">Executing a `PreparedStatement`</li>
<li class="fragment fade-up">Reusing a `PreparedStatement`</li>
</ul>
-

`PreparedStatement` is a subclass of `Statement`

-

PreparedStatement: __Benefits__

* Improves performance of an application
* Easy to set SQL parameter values
* Prevents **SQL Injection** Attacks

-
###PreparedStatement: example


Say you have the following query: 
```SQL
SELECT * FROM Employees WHERE Salary < 10000 AND department_id = 50;
```

It can be re-written using place-holders for the values:
```SQL
SELECT * FROM Employees WHERE Salary < {some value}
	AND department_id = {some value};
```
to become: 
```SQL
SELECT * FROM Employees WHERE Salary < ? AND department_id = ?;

```

-
###PreparedStatement: example (continued)

To use SQL statements that takes variable value, a `?` is used as a parameter place holder. Now, we can use the same statement with different values each time we execute it:


```Java
PreparedStatement pstmt = conn.prepareStatement(
	"SELECT * FROM Employees WHERE Salary < ? AND department_id = ?");
```
-
###PreparedStatement: example (continued)

To bind the values of the parameters, use:

`setXxx()` - where `Xxx` is the data type of the value,

__for example:__

* `setInt(P1,P2)`
* `setString(P1, P2)`
* `setDouble(P1, P2)`

_note: P1 = Position (1 based), P2 = Value_

-

Examples of setting the values to a prepared statement

```Java 
PreparedStatement pstmt = conn.prepareStatement(
	"SELECT * FROM Employees WHERE Salary < ? AND department_id = ?");

pstmt.setDouble(1, 1000)
pstmt.setInt(2, 50)
```

-
#### Process to execute a PreparedStatement

1. Establish a Connection with the Database
2. Prepare the Statement using Parameter placeholder(?)
3. Set the values for Parameters
4. Execute the Statement

-

#### Inserting Data
General Example SQL Insert statement
```SQL
INSERT INTO TableName VALUES(value1, value2, ...);
```

-
Practical Example of SQL Insert statement

```SQL
INSERT INTO Employees VALUES(
		435, momo, momo@gmail.com,'2019-03-22', 50000);
```

-
Example of Prepared Statement
```
PreparedStatement pstmt = conn.prepareStatement("
	INSERT INTO NewEmployees
	VALUES (?,?,?,?,?)");
```

-
#### Update Data

Example Prepared Statement SQL

```
UPDATE Employees SET salary = ? WHERE Employee_Id = ?
```

-
#### Removing Data

Example Prepared Statement SQL

```
DELETE FROM Employees WHERE Employee_Id = ?
```

-
### Prepared Statement Summary

* Statement
* ResultSet
* Scrollable ResultSet & Updatable ResultSet
* PreparedStatement
* CRUD operations using prepared statements


-
-

## Data Access Object(DAO) Pattern

* Provides abstraction between JDBC and the rest of the code
* Can be just abstraction or a true object
* Most use data transfer objects(DTOs) with data access objects(DAOs)/abstract
* Provides a clear separation of concerns in code

-

### DTO

* Data Transfer Object
* Provides single-domain data
* Should fully encapsulate object and/or contain subobjects
* Should be output and input of a single DAO

-

### DAO

* Usually leverages a common interface
* Concrete implementation reacts on a single-data domain
* Can and usually supports multiple tables
* Encapsulation of complex joins and aggregations

-

### DAO Factory

* Often used with DAOs
* Provides ability to leverage common paths for basic CRUD operations
* Loses value when you have a bunch of custom methods
* Object-oriented programming (OOP) pure

-

### Repository Pattern

* Single-table access per class
* Instead of joining in the database, you'll join in code

-

* Repository pattern allows sharding of database
* You can store one piece of data in a separate database to facilitate distribution



-
-

 <img src="https://zipcoder.github.io/reveal-slides.tcus/img/bunnies/fluffy-bunny.jpg">
