# Intro to JDBC

-

## Objectives

* Introduce JDBC
* Connect to SQL Database
* Perform CRUD operations

-

## Introduction

JDBC is an API for the Java programming language that defines how a client may access a Database.

Part of Java SE, found in java.sql and javax.sql packages

Java Applicaation <--> JDBC <--> JDBC Driver <--> DB

(Graphic needed)

-

### Architecture of JDBC

* JDBC API - Application to JDBC Manager Connection
* JDBC Driver API - JDBC Manager to Driver Connection

(Graphic Needed)

-

### Driver Manager

The basic service for managing a set of jdbc drivers

(Graphic Needed)

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

* It is very easy to use
* Almost any database is supported

__Disadvatages__

* Performance will not be efficient
* ODBC Driver needs to be installed
* Type 1 drivers are not portable
* Not suitable for Applets

-

#### Type 2 : Native-API driver
__Advantages__

* Faster than Type 1 Driver

__Disadvatages__

* Client Side Llibrary is not available for all databases
* Vendor Client Library needs to be installed
* It is a Platform Dependent
* Not Thread Safe

-

#### Type 3 : Network-Protocol driver (Middleware driver)
__Advantages__

* No additional library installation is required on client system
* No changes are required at client for any DB
* Supprts Caching of Connection, Query Results, Load Balancing Logging and Auditing etc.
* A Single Driver can handle any database provided the middleware supports it

__Disadvatages__

* Performance will be slow
* Requires Database-specific codeing
* Maintenance of Network Protocol driver becomes costly

-

#### Type 4 : Database-Protocol driver (Pure Java driver)
__Advantages__

* Platform Independent
* No intermediate format is required
* Application connects directly to the database server
* Performance will be very fast
* JVM manage allaspects

__Disadvatages__

* Drivers are database dependent

-

#### Overview

* If you are accesssing one type of database such as Oracle, SQL Server, MYSQL etc. then the preferred driver is 4
* If your jJava application is accessing multiple types of databases at the same time, type 3 is the preferred driver.
* Type 2 drivers are useful in situations where a type 3 or type 4 driver is not available yet for your database
* The type 1 driver is not considered a deployment-level driver and it is typically used for development and testing purposes only

-

### Summary

* What is the JDBC
* Architecture of JDBC
* Role of Driver Manager
* Understanding JDBC Driver Types

-
-

## Connecting to a MYSQL Database

First we must get the mysql connecter for java. This can be done using the pom.xml file.

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.10</version>
</dependency>
```

-

The DriverManager object will need 3 pieces of information to create a connection to a database

```
    static String username = "user name here";
    static String password = "user password here";
    static String dbUrl = "jdbc:mysql://localhost:3306/someDb";

```

-

Calling the _.getConnection()_, passing the previous aurgments with hopefully return a Connection object the references the connection to the database

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

#### Exception Handling

JDBC methods throw SQLException

SQLException extendsed from the Exception class

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
* Handling JDBC Execptions

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

Two Interfaces for executing static sql statements

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

### Steps for Development Process

1. Establish Connection to a Database
2. Create Statement Object
3. Execute SQL Query
4. Process the ResultSet

-

#### ResultSet Interface

A ResultSet is a multidimensional array that return a database statement

Java.Sql.ResultSet interface represents the result set of a database query

When the ResultSet is first returned, the starting cursor position is before the first row of data

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

-

#### Scrollable ResultSets

__ResultSet Types__

* Type_ Forward_Only
* Type_ Scroll_Insensitive
* Type_ Scroll_Sensitive

__ResultSet Concerrency Type__

* Concur_ Read_Only
* Concur_Updatable

-

#### Update Methods

Updatable ResultSet allows modification to data in a table through the ResultSet

* Each Update method has two versions
	1. One that takes in a column name
	2. One that takes in a colomn index

To update a String Column of the current row

* public void updateString(int columnIndex, String s)
* public void updateString(String columnName, String s)

_All update methods throw SQLException_

Making these changes only changes the data in the ResultSet Object not the underlying Database.

The following will make changes to the actual

* updateRow()
* deleteRow()
* refreshRow()
* cancelRowUpdates()
* insertRow()

-

### Prepared statements

* What is a PreparedStatement
* How to create a PreparedStatement
* Setting Parameters of a PreparedStatement
* Executing a PreparedStatement
* Reusing a PreparedStatement

PreparedStament is a subclass of Statement

__Benefits__

* Improves performance of an application
* Easy to Set SQL parameter values
* Prevents SQL Dependency Injection Attacks

```
Select * from Employees Where Salary < 10000 and department_id = 50;
```

```
Select * from Employees Where Salary < {some value} and department_id = {some value};
```

```
Select * from Employees Where Salary < ? and department_id = ?;

```

To use SQL statements that take value a _?_ is used as a parameter place holder. Now we can use the same statement with different values each time we execute

__Example__

```
PreparedStatement pstmt = conn.createStatement("select * from Employees where salary < ? and and department_id = ?");
```

To bind the values of the parameters us:

setXxx() - where Xxx is the data type of the value

__Examples__

* setInt(P1,P2)
* setString(P1, P2)
* setDouble(P1, P2)

_note: P1 = Position (1 based), P2 = Value_

Examples of setting the values to a prepared statement

```
PreparedStatement pstmt = conn.createStatement("select * from Employees where salary < ? and and department_id = ?");

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

```
Insert into TableName values(value1, value2, ...);
```

Practical Example of SQL Insert statement

```
Insert into Employees values(435, momo, momo@gmail.com,'2019-03-22', 50000);
```

Example of Prepared Statement
```
PreparedStatement pstmt = conn.prepareStatement(insert into NewEmployees values (?,?,?,?,?));
```

-

#### Update Data

Example Prepared Statement SQL

```
update Employees set salery = ? where Employee_Id = ?
```

-

#### Removing Data

Example Prepared Statment SQL

```
delete form Employees where Employee_Id = ?
```

-

### Stored Procedure Summary

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
* Provides a clear seperation of concerns in code

-

### DTO

* Data Transfer Object
* Provides single-domain data
* Should fully encapsulate object and/or contain subobjects
* Sould be output and input of a single DAO

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
* 