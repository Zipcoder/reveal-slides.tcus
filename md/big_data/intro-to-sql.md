# Intro to SQL and Relational Databases

-

### Overview

- Definitions
- Connecting to a database
- Creating a schema with tables
- Seeding a database
- SELECTING data
- Analytics/Processing
- Relationships
- Putting it into code

-


### What is a database?

A database is an organized collection of data. A database will usually have some sort of api to query the data. Using a Structured Query Language (SQL) will allow you to interact with the data in a predictable way. A query in this case is a statement you ask a database. A properly made database will be able to answer a query with all relevent information.

-
-

# Connecting to a database

-

### MySQL

MySQL is a popular Relational Database Management System (RDBMS). It runs on most systems, so it can be run directly on your own machine. It is often preferred to run it on a Docker instance running LINUX. 

-

### MySQL

In order to connect to a MySQL instance, you need to know a few things:

* Host - this is usually a url or ip address for a server
* Port - Port listening for a connection (defaults to 3306)
* Username - The MySQL user you are connecting with
* Password - The password for the user (This could be blank)

-
### MySQL: Installing via HomeBrew

Homebrew is a package manager for Mac that is one of the most common ways to install an app locally.

To install Homebrew, open **Terminal** and run:

```Shell
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```
Note: Homebrew will download and install Command Line Tools for Xcode as part of the installation process, which may take a while.


-
### MySQL: Installing via HomeBrew (continued)

Then install MySQL using Homebrew:

```Shell
$ brew install mysql

```

Install brew services:

```Shell
$ brew tap homebrew/services 
```

-
### MySQL: Installing via HomeBrew (continued)

Load and start the MySQL service:

```Shell
$ brew services start mysql 
```

Expected output: 

```Shell
Successfully started mysql (label: homebrew.mxcl.mysql)

```

Open Terminal and execute the following command to set the root password:

```Shell
mysqladmin -u root password 'yourpassword'

```

Now your MySQL server is ready.

-
### Installing MySQL on MacOS Catalina and later: 

Catalina threw us a whammy! 

To install MySQL successfully on Catalina, first make sure no other versions are running on your machine: 

```Shell
$ sudo ps ax | grep mysql
```

If you see more lines in the response than the one that contains `grep mysql`, then you'll need to locate the `mysql` folders and remove them. 

To search for them, press `⌘` and the space bar and enter `mysqladmin` into the prompt, followed by selecting "Show All In Finder" in the left rail.  


-
### Installing MySQL on MacOS Catalina and later (continued): 

Once all the other versions are removed, reboot your computer and install the DMG package from: 
<a href="https://dev.mysql.com/downloads/mysql/" target="_blank">https://dev.mysql.com/downloads/mysql/</a>

Once you have successfully installed the package, add the mysql binary to the bash profile: 

```Shell
$ nano ~/.bash_profile
```

At the bottom of the file, add th following line: 

```Shell
export PATH="$PATH:/usr/local/mysql/bin"
```
Quit **Terminal**, and restart it, opening a new **Terminal** window. You should now be able to connect to **MySQL** successfully.


-
### MySQL: Installing via Docker

Another easy way to install a local MySQL instance on any machine is with a **Docker** container. The following line will start up an instance: 

```Shell
$ docker pull mysql
$ docker run --name local-mysql -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 -d mysql

```

This will allow you to connect using the following 

```Shell
Host: localhost
Port: 3306
User: root
Password: password

```

-

### MySQL

Once installed, you can connect to your MySQL server via the command line: 

```Shell
$ mysql -u root -p
```

You will be prompted to enter the password: 

```Shell
Enter password: 
```

Once the password is entered correctly, you will be connected, and a MySQL prompt will appear:

```SQL
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

```
To disconnect, type `exit`.
-

### MySQL

Once you have connected, it is advised for security reasons that you create a new user. The current user is the root user which will always have all privledges and it is insecure to use it for general purposes. To make a new user and give them permissions to do anything (ergo an admin user) you may use the following SQL Script

```Shell
GRANT ALL PRIVILEGES ON *.* TO 'new_user'@'localhost' IDENTIFIED BY 'password';

```

This will create a user with username `new_user` and password `password`. Note if you forget the root password and this password you will not have a way of resetting this.

-

### MySQL

* Created user has all the same powers as root user
* Keep track of who is running particular queries
* Possible to create users that can only read data
* *Keep everyone's permissions to  minimum...* even yours

-
-

# Creating a schema with tables

-

### Schemas and Tables

Lets say we are designing a database for our backend developers. They are creating an app for teachers to keep track of their student's assignments. We'll also wanna keep meta data on the teachers. Their application has Students, Teachers, Labs, and Submissions. Let's try to go through designing this database

First we'll need to make a schema. To do this we can run the following script

```SQL
CREATE SCHEMA zipcode;

```

-

### Schemas and Tables

What have we just done? _We created a SCHEMA_

- Created a home for all data related to an application
- Allowed ourselves to manage multiple applications in the same database without worrying about collisions



-
-
### Schemas and Tables: SHOW DATABASES

To display the databases that exist on your server, use the `SHOW DATABASES` command:

```SQL
SHOW DATABASES;
```
Expected result: 

```SQL
+-----------------------+
| Database              |
+-----------------------+
| information_schema    |
| mysql                 |
| performance_schema    |
| sys                   |
| zc_de001              |
| zipcode               |
+-----------------------+
6 rows in set (0.00 sec)

```
 


-
-
### Schemas and Tables: USE
- Select the database (schema) so that you can run queries and commands against it. 
- If no database has been selected, you must specify the schema in each query

```SQL 
USE zipcode;
```

Expected result: 

```SQL
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```


-
-
### Schemas and Tables: CREATE TABLE
- Creates a table.
- Must be done before putting any data in.
- This is where you define what your table will look like.
- Can be modified later by doing an ALTER TABLE.
- Defines the table name, column names, datatypes, and special rules / constraints.
- Also how you link to other tables.

-

### Schemas and Tables: CREATE TABLE syntax
```SQL
CREATE TABLE table_name (
  column1 datatype,
  column2 datatype,
  column3 datatype
);
```

-

### Schemas and Tables: CREATE TABLE example

Teachers have a name and a specialty. We are going to create a table with these fields as well as a unique identifier. To create a a table we use the `CREATE TABLE` command. Let's make the Teacher Table

```SQL
CREATE TABLE zipcode.teachers
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    specialty ENUM('FRONT END', 'MIDDLE TIER', 'DATA TIER')
);

```

In this query we create each column followed by the properties of the columns. We are using the `INTEGER`, `VARCHAR`, and `ENUM` data types

-

### Schemas and Tables: CREATE TABLE example 

Next, let's make a student table. The students will have a name, and a classroom as well notes from the teachers on a particular student

```SQL
CREATE TABLE zipcode.students
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    classroom TINYTEXT,
    notes TEXT
);

```

In this query, we create each of these columns and are using `INTEGER`, `VARCHAR`, `TINYTEXT`, and `TEXT`.

-

### Schemas and Tables: CREATE TABLE example 

Lastly we need an assignment table. This should just have the assignment name and a link to the assignment

```SQL
CREATE TABLE zipcode.assignments
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name TINYTEXT NOT NULL,
    URL CHAR(255) NOT NULL
);

```

Here we are creating a table similar to the last two, but making the URL a fixed length `CHAR` field and making that unique. Why?

-

### Schemas and Tables

Lastly, we'll create the **teacher_meta** table. In the meta table, the developers want to keep track of the number of years a teacher has worked here, and the room number of the teacher's office.

```SQL
CREATE TABLE zipcode.teacher_meta
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    room_number TINYINT UNSIGNED,
    years TINYINT UNSIGNED
);
```

Here, we use the `TINYINT` column and make those unsigned


-
-

### Schemas and Tables: SHOW TABLES
To display the tables in your chosen database (schema), use the `SHOW TABLES` command:

```SQL
SHOW TABLES;
```

Expected result: 

```SQL
+--------------------+
| Tables_in_zipcode  |
+--------------------+
| assignment_student |
| assignments        |
| students           |
| teacher_meta       |
| teachers           |
+--------------------+
5 rows in set (0.00 sec)
```


-

### Schemas and Tables: DESCRIBE

To display the details of a table settings, use the `DESCRIBE` command: 

```SQL
describe teachers;
```
Expected result:

```Shell
+------------+---------------------------------------------+------+-----+---------+----------------+
| Field      | Type                                        | Null | Key | Default | Extra          |
+------------+---------------------------------------------+------+-----+---------+----------------+
| id         | int(11)                                     | NO   | PRI | NULL    | auto_increment |
| specialty  | enum('FRONT END','MIDDLE TIER','DATA TIER') | YES  |     | NULL    |                |
| first_name | varchar(25)                                 | NO   |     | NULL    |                |
| last_name  | varchar(25)                                 | NO   |     | NULL    |                |
+------------+---------------------------------------------+------+-----+---------+----------------+
4 rows in set (0.00 sec)
```

-
-

### Schemas and Tables: ALTER TABLE

Now we have tables, but we have learned that teachers are actually going to have a first name and last name, and the developers have said that they want these to be two separate fields. Before we continue, let's `ALTER` this table.

-

### Schemas and Tables: ALTER TABLE Syntax

```SQL
ALTER TABLE table_name
    [alter_specification [, alter_specification] ...]
```

-
### Schemas and Tables: ALTER TABLE ADD

`ALTER TABLE ADD` statement appends a new column to a table.

In this statement, you must specify the following:
    - the name of the table in which you want to add the new column
    - the name of the column, data type, and constraint if applicable

```SQL
ALTER TABLE table_name
ADD column_name data_type column_constraint;

```

-
### Schemas and Tables: ALTER TABLE ADD

If you want to add multiple columns to a table at once using a single ALTER TABLE statement, specify a comma-separated list of columns that you want to add to a table after the ADD clause:

```SQL
ALTER TABLE table_name
ADD 
    column_name_1 data_type_1 column_constraint_1,
    column_name_2 data_type_2 column_constraint_2,
    ...,
    column_name_n data_type_n column_constraint_n;

```

-
### Schemas and Tables: ALTER TABLE CHANGE

Sometimes, you will need to rename a column and retain its data. For this, you will use `CHANGE` clause, specifying the following:
    - the name of the table in which you want to rename the column.
    - the name of the column that you want to rename and its desired settings.

```SQL
ALTER TABLE table_name CHANGE old_name new_name data_type column_constraint;

```

-
### Schemas and Tables: ALTER TABLE CHANGE

If you want to chang multiple columns in the same statement:

```SQL
ALTER TABLE table_name 
    CHANGE old_name new_name data_type column_constraint, 
    CHANGE old_name2 new_name3 data_type column_constraint,   
    CHANGE old_name3 new_name3 data_type column_constraint ;

```

-
### Schemas and Tables: ALTER TABLE DROP COLUMN

Sometimes, you need to remove one or more columns from a table. To do this, you use the `ALTER TABLE DROP COLUMN` statement. In this statement, you must specify the following:
    - the name of the table from which you want to delete the column.
    - the name of the column that you want to delete.

```SQL
ALTER TABLE table_name
DROP COLUMN column_name;

```

-
### Schemas and Tables: ALTER TABLE DROP COLUMN - a note

Sometimes a **constraint** on the column will require additional steps before dropping it. For example, if the column that you want to delete has a `CHECK` **constraint**, you must delete the **constraint** first before removing the column. 

We'll more talk about adding and dropping contraints later. 

-
### Schemas and Tables: ALTER TABLE DROP COLUMN Syntax

To delete multiple columns at once, specify columns that you want to drop as a list of comma-separated columns in the `DROP COLUMN` clause:

```SQL
ALTER TABLE table_name
DROP COLUMN column_name_1, DROP column_name_2,...;
```

Now, back to our updates as requested by the developers.

-

### Schemas and Tables: ALTER TABLE example

For our change according to developer request, we are going to `ADD` **firstname** and **lastname** columns. Then, we will `DROP` the **name** column. 

```SQL
ALTER TABLE zipcode.teachers 
    ADD 
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL;
    
ALTER TABLE zipcode.teachers DROP name;
```

This will add our new columns to the `teachers` table, in the order they were entered, and remove the `name` column completely.

-
### Schemas and Tables: ALTER TABLE example

After our updates, the developers have requested that we change the **firstname** and **lastname** columns to **first_name** and **last_name**. 

```SQL
ALTER TABLE teachers CHANGE firstname first_name varchar(255);

```

This change the names of the columns, while leaving it's underlying settings, and their content intact in the `teachers` table.



-
-

# Seeding a Database

-

### Seeding a Database: INSERT

Our database is set up for our devs to use, but they've asked us to create some mock data for them to demo their app. To do this we'll have to `INSERT` some data.

Let's start by inserting a few teachers.

-

### Seeding a Database: INSERT

Puts data into a table.

Inserts will generally have 3 parts

- Reference to the table you want to insert into
- The columns you want to fill
- The values to fill those columns with
    - Must provide all required values.
      - This means columns that specify the following:
        - `NOT NULL`
        - Do not have a `DEFAULT`
        - Are not `AUTO_INCREMENT` columns
    - If `NULL` values are allowed, those columns can be excluded in the `INSERT`.

Let's create a teacher, and then that teacher's meta info.

-
### Seeding a Database: INSERT syntax -- No column names
- Without specifying columns, you must make sure that every column is accounted for in the insert.
```SQL
INSERT INTO table
VALUES(column1_value, column2_value);
```

-
### Seeding a Database: INSERT example -- No column names
```SQL
INSERT INTO zipcode.teachers
    VALUES ('FRONT END', 'John', 'Smith');
```

-
### Seeding a Database: INSERT syntax -- With column names
```SQL
INSERT INTO table(column1_name, column2_name)
VALUES(column1_value, column2_value);
```

-

### Seeding a Database: INSERT example -- With column names

```SQL
INSERT INTO zipcode.teachers (first_name, last_name, specialty)
    VALUES ('John', 'Smith', 'FRONT END');

```

Here we are creating a new row in the teachers table. The first_name is set to John, the last_name is set to Smith, and the specialty is set to FRONT END. Note that the order of the columns we listed will be the order we list the values. If there is a different number of columns and values, then this will throw an error. Also note that since the id on this table is set to auto increment, it will automatically be filled in with the id of 1. 

-

### Inserts

We now have one teacher in this table and we can add a few more, but instead of running them one by one, we can also just add many at the same time.

```SQL
INSERT INTO zipcode.teachers (first_name, last_name, specialty)
    VALUES ('Tabitha', 'Schultz', 'MIDDLE TIER'),
      ('Jane', 'Herman', 'DATA TIER');
```

-

### Inserts

With the ability to insert data, we can also start populating the other tables

```SQL
INSERT INTO zipcode.teacher_meta (teacher_id, years, room_number)
    VALUES (1, 3, 2),
      (2, 3, 2),
      (3, 10, 1);
```

```SQL
INSERT INTO zipcode.students (name, classroom, notes)
  VALUES  ('Linnell McLanachan', '1A', 'Likes Data'),
	('Lorianna Henrion', '1A', 'Loves Data'),
	('Corena Edgeson', '1A', 'Cannot get enough of data'),
	('Archaimbaud Lougheid', '2A', 'Would rather do nothing other than sit down in front of a mountain of data and read through it like a book. SERIOUSLY needs to seek help about this because there is no way it is healthy for this person to like data any more than they do'),
	('Dun Pettet', '2A', NULL ),
	('Hymie Parrington', '2A', 'Enjoys the Star Wars prequels');
	
```

-

### Inserts

```SQL
INSERT INTO zipcode.assignments (name, URL, teacher_id)
  VALUES ('Pokemon Lab', 'https://github.com/Zipcoder/PokemonSqlLab', 3),
  ('Poll App', 'https://github.com/Zipcoder/CR-MacroLabs-Spring-QuickPollApplication', 3),
  ('Sum or Product', 'https://github.com/Zipcoder/ZCW-MicroLabs-JavaFundamentals-SumOrProduct', 2);
  
```

```SQL
INSERT INTO zipcode.assignment_student (assignment_id, student_id)
    VALUES (1, 1), (1, 2), (1,3), (1,4), (1,5), (1,6), (2, 1);
    
```

-

### Updates

Updating data is very similar to inserting, but we will use an `UPDATE` clause instead. For this we will need to specify a table we want to update, then a `SET` clause to specify how to change a field or fields.

Our devs inform us that they want a way to increment the number of years each teacher has worked here. Let's see how we would write that update statement

-

### Updates

```SQL
UPDATE zipcode.teacher_meta
SET years = years+1;
```

Here we are able to increment the years by setting years = to whatever it's own value is plus one. This works because SQL will go row by row. In each row, the years variable is set to whatever the value is in that particular row. 

-
-


### Stored Procedures

Now that we know how to do inserts and updates, it may be useful for us to create a stored procedure for something that will be done regularly. The devs have asked us to put this update statement in a Stored Procedure. 

```
DELIMITER //
CREATE PROCEDURE zipcode.increment_years_experience ()
BEGIN
  UPDATE zipcode.teacher_meta
    SET years = years+1
    WHERE ID <> 0;
END //
DELIMITER ;
```

```
CALL increment_years_experience;
```

This procedure can be called instead of a developer trying to write their own update statement. This gives us control over the data and ensures it's quality remains up to a standard. 

-
-

# Viewing the Data

-

### Selects

We've now finished all the work we need to do for the devs and are free to explore our own database and come up with some queries that will be able to answer some questions about the students. These updates will keep our boss happy and help our teachers know what they need to  do to give students the highest quality of education. 

-

### Selects

In order to get this done we'll have to `SELECT` data out of our database. A `SELECT` statement will generally have at least two parts. 

- A Select clause where we say the columns we want to see 
- A FROM clause where we specify which table to pull that data from.

-

```
SELECT id, first_name, last_name, specialty 
FROM zipcode.teachers;
```

| ID | First Name | Last Name |  Specialty  |
|:---|:-----------|:----------|:------------|
| 1  | John       | Smith     | FRONT END   |
| 2  | Tabitha    | Schultz   | MIDDLE TIER |
| 3  | Jane       | Herman    | DATA TIER   |


-

### Where Clause

We've been informed by a higher up that there is a very important project that needs a lead. Anyone who can work on front end development will be able to help immensely. Let's try and `SELECT` from teachers again, but this time let's add a `WHERE` statement to ensure we only pull a teacher with the FRONT END specialty.


-

### Where Clause

```
SELECT id, first_name, last_name, specialty
FROM zipcode.teachers
WHERE specialty='FRONT END';
```

| ID | First Name | Last Name |  Specialty  |
|:---|:-----------|:----------|:------------|
| 1  | John       | Smith     | FRONT END   |

-

### Limit and Order Clauses

This will return a single row with the teacher John Smith who is our only FRONT END specialist. You tell the higher ups that you think John would be up to the task. They say great, but ask who can take over that teacher's spot. We'll wanna choose the teacher with the most experience, who isn't John. 

-

### Limit and Order Clauses

We check and see that John's id is 1, so we'll keep that in mind. Next we think of how we can find the teacher with the highest years of experience. to do this, we may use an `ORDER BY` statement. This kind of statement will take a list of fields that we will sort a table on and the direction we want to sort them. The two directions are `ASC` and `DESC`

-

### Limit and Order Clauses

```
SELECT id, room_number, years, teacher_id FROM zipcode.teacher_meta
WHERE teacher_id != 1
ORDER BY years DESC;
```

We specify DESC here so that the table will be ordered by highest years to lowest years. This works but we do want to just find the one top teacher id. To do this we can use a `LIMIT`. The Limit clause will take the number of items you want to reutrn

-

### Limit and Order Clauses

```
SELECT teacher_id FROM zipcode.teacher_meta
WHERE teacher_id != 1
ORDER BY years DESC
LIMIT 1;
```
| teacher_id |
|:----------:|
| 3          |

Select from the teachers table where id is 3 and find that Jane is the best person to take the extra classes on.

-
-

# Relationships

-

### Joins

Viewing the data in one table can be useful, but often we'll want to see mutiple tables' data together. To do this we use a `JOIN`.

The Join clause will have two parts

- The table to join to
- The fields to compare

-

### Joins - One to One

Let's do our first join to see the number of years each teacher has worked here.

```
SELECT t.first_name, t.last_name, tm.years FROM
  zipcode.teachers t
JOIN zipcode.teacher_meta tm
  ON t.id = tm.teacher_id;
```
| first_name | last_name | years |
|:-----------|:----------|------:|
| John       | Smith     | 4     |
| Tabitha    | Schultz   | 4     |
| Jane       | Herman    | 11    |

-

### Joins - One to Many

Next lets add a join to see which teachers wrote each assignment. This time, since some teachers may have written more than one assignment, we may see some duplication in the results.

-

```
SELECT t.first_name, t.last_name, a.name, a.URL
FROM zipcode.teachers t
JOIN zipcode.assignments a
  ON t.id = a.teacher_id;
```

| first_name | last_name | name           | URL                                                                     |
|:-----------|:----------|:---------------|:------------------------------------------------------------------------|
| Jane       | Herman    | Pokemon Lab    | https://github.com/Zipcoder/PokemonSqlLab                               |
| Jane       | Herman    | Poll App       | https://github.com/Zipcoder/CR-MacroLabs-Spring-QuickPollApplication    |
| Tabitha    | Schultz   | Sum or Product | https://github.com/Zipcoder/ZCW-MicroLabs-JavaFundamentals-SumOrProduct |


-

### Joins - Many to Many

Lastly lets see which assignments have been given to each student.

For this relationship we must first join the pivot table, then join the destination table. 

-

```
SELECT s.name, a.name
FROM zipcode.students s
JOIN zipcode.assignment_student a_s
  ON a_s.student_id = s.id
JOIN zipcode.assignments a
  ON a_s.assignment_id = a.id;
```
| name                 | name        |
|:---------------------|:------------|
| Linnell McLanachan   | Pokemon Lab |
| Lorianna Henrion     | Pokemon Lab |
| Corena Edgeson       | Pokemon Lab |
| Archaimbaud Lougheid | Pokemon Lab |
| Dun Pettet           | Pokemon Lab |
| Hymie Parrington     | Pokemon Lab |
| Linnell McLanachan   | Poll App    |



-
-

# Analytics

-

### Aggregates

Aggregates are functions that allow you to run a function down a column rather than on each particular row. Some of these include

- COUNT
- MAX
- MIN
- SUM

-

### Aggregates - Count

Count will give you the total number of rows in a table based on a column. 

```
SELECT COUNT(*) as "count"
FROM teachers;
```
| count |
|:-----:|
| 3     |

Count does not consider the values in any column. It will simply add one for each row.

-

### Aggregates - Max/Min

Max and min will give you the largest or smallest value of a column in any row.

```
SELECT MAX(years) as "max", MIN(years) as "min"
FROM zipcode.teacher_meta
```

| max | min |
|:----|:----|
| 11  | 4   |

Max and Min will consider data type when doing this sort. Text will sort lexographically and numbers will sort numerically.

-

### Aggregates - Sum

Sum will take the value of a numeric column and add together all of the values.

```
SELECT SUM(years) as "sum"
FROM zipcode.teacher_meta
```

| sum |
|:---:|
| 19  |

-

### Aggregates - Group Concat

Group Concat will take a column and concatenate all the rows. 

```
SELECT GROUP_CONCAT(' ', first_name, ' ', last_name) as "group_concat"
FROM zipcode.teachers;
```

| group_concat                              |
|:------------------------------------------|
|  John Smith, Tabitha Schultz, Jane Herman |

-

### Group By 

Sometimes you want to take an aggregate of rows but you don't want to indiscriminately aggregate all rows. You want to group some rows together and aggregate those.

Right now in order to do that you could try using a `WHERE` clause, but you'd have to know some information about the rows beforehand. Instead we may group a column with a Group By.

Lets write a query to list teachers who have equivilent experience together.

-

### Group By

```
SELECT GROUP_CONCAT(' ', first_name, ' ', last_name) as "teachers", years
FROM zipcode.teachers t
JOIN zipcode.teacher_meta tm
  ON t.id = tm.teacher_id
GROUP BY tm.years;
```
| teachers                    | years |
|:----------------------------|:-----:|
| John Smith, Tabitha Schultz | 4     |
| Jane Herman                 | 11    |


-

### Having

A Having clause can be used to filter the results of a query. This is similar to the Where clause, but it works only with fields that are Aggregates.

Lets Write a query to find all students who have been assigned more than one assignemnt.

-

### Having

```
SELECT s.name, COUNT(a_s.assignment_id) as "assignments given"
FROM zipcode.students s
JOIN zipcode.assignment_student a_s
  ON s.id = a_s.student_id
GROUP BY s.name
HAVING COUNT(a_s.assignment_id) > 1;
```

| name               | assignemnts given |
|:-------------------|:-----------------:|
| Linnell McLanachan | 2                 |

-

<img src="../img/bunnies/t1-505860-lacie_bunny.jpg">

DBA corgis hope you're `HAVING` a blast!
