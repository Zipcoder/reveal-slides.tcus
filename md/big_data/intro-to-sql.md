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
-

### What is a databse?

A database is an organized collection of data. A database will usually have some sort of api to query the data. Using a Structured Query Language (SQL) will allow you to interact with the data in a predictable way. A query in this case is a statement you ask a database. A properly made database will be able to answer a query with all relevent information.

What makes a database relational though?

-

### Indexing

"
A database index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure
"

Basically, you put a sepcial tag on a piece of data so that you can find it faster. This requires extra work and takes more space on disk, but will help speed up your databse.

-

### Indexing

Most sql data sets will have columns based on the fields of an item and rows for each of the individaul items. When written out it sort of looks like an excel spreadsheet.

| First Name | Last Name | Age | Gender |
|:-----------|:----------|:----|:-------|
| Leon       | Hunter    | 24  | Male   |
| Wilhem     | Alcivar   | 23  | NULL   |
| Nhu        | Nguyen    | NULL| Female |

-

### Indexing

Issue: Let us say we want to find out Leon's age. We might tell our databse to return all the info related to the row with the first name of Leon. For now that would work since we only have one Leon in the databse, but what if we hired another Leon? We could ask for only Leon Hunter, but we really cannot rely on this data always being unique. 

| First Name | Last Name | Age | Gender |
|:-----------|:----------|:----|:-------|
| Leon       | Hunter    | 24  | Male   |
| Leon       | Smith     | 24  | Male   |

-

### Indexing

Solution: Add a unique id to each row. 

| ID | First Name | Last Name | Age | Gender |
|:---|:-----------|:----------|:----|:-------|
| 1  | Leon       | Hunter    | 24  | Male   |
| 2  | Wilhem.    | Alcivar   | 23  | NULL   |
| 3  | Nhu.       | Nguyen    | NULL| Female |

We can denote this id as the `PRIMARY KEY` which will make this an index. Searching for a row by its id will not only help us get only the data we want, but it will also be faster by a mesurable degree

-

### Indexing

Lets say we keep a list of phone numbers now: 

| ID | Phone Number | Phone Owner |
|:---|:-------------|:------------|
| 1  | 555-321-4547 | Wilhem      |
| 2  | 555-221-4548 | Leon        |
| 3  | 555-782-4549 | Nhu         |

Same issue as before. That owner there refers to only one person now, but how can we make sure that we match it to the correct person?

-

### Indexing

Solution: Use the person's unique id to identify who this number belongs to

| ID | Phone Number | Phone Owner ID |
|:---|:-------------|:---------------|
| 1  | 555-321-4547 | 2              |
| 2  | 555-221-4548 | 1              |
| 3  | 555-782-4549 | 3              |

To find out who owns the phone number we would take that id and search for it in the Person list that we made above. This is a relationship. Relational data uses these kinds of relationships

-

### Relationships

Say we have tables A and B.

One to One - One item in table A has one item in table B. One item in table B has one item in table A

One to Many - One item in table A has many items in table B. One item in table B has one item in table A

Many to Many - One item in table A has many items in table B. One item in table B has many items in table A

-
-

# Connecting to a database

-

### MySQL

Mysql is a popular Relational Database Management System (RDBMS). It runs on most systems so it can be run directly on your own machine, but its best to run it on a docker instance running linux. 

-

### MySQL

In order to connect to a MySQL instance, you need to know a few things

* Host - this is usually a url or ip address for a server
* Port - Port listening for a connection. Defaults to 3306
* Username - The MySQL user you are connecting with
* Password - The password for the user (This could be blank)

-

### MySQL

The easiest way to install a local MySQL instance on any machine is with a docker container. The following line will start up an instance

```
$ docker run --name local-mysql -e MYSQL_ROOT_PASSWORD=password\
-p 3306:3306 -d mysql:tag
```

This will allow you to connect using the following 

```
Host: localhost
Port: 3306
User: root
Password: password
```

-

### MySQL

Once you have connected, it is advised for security reasons that you create a new user. The current user is the root user which will always have all privledges and it is insecure to use it for general purposes. To make a new user and give them permissions to do anything (ergo an admin user) you may use the following SQL Script

```
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

```
CREATE SCHEMA zipcode
```

-
### Schemas and Tables

What have we just done? _We created a SCHEMA_

- Created a home for all data related to an application
- Allowed ourselves to manage multiple applications in the same databse without worrying about collisions

-

### Schemas and Tables

Teachers have a name and a specialty. We are going to create a table with these fields as well as a unique identifier. To create a a table we use the `CREATE TABLE` command. Let's make the Teacher Table

```
CREATE TABLE zipcode.teachers
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    specialty ENUM('FRONT END', 'MIDDLE TIER', 'DATA TIER')
);
CREATE UNIQUE INDEX teachers_id_uindex ON zipcode.teachers (id);
```

In this query we create each column followed by the properties of the columns. We are using the `INTEGER`, `VARCHAR`, and `ENUM` data types

-

### Schemas and Tables

Next let us make a student table. The students will have a name, and a classroom as well notes from the teachers on a particular student

```
CREATE TABLE zipcode.students
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    classroom TINYTEXT,
    notes TEXT
);
CREATE UNIQUE INDEX students_id_uindex ON zipcode.students (id);
```

In this query we create each of these columns and are using `INTEGER`, `VARCHAR`, `TINYTEXT`, and `TEXT`.

-

### Schemas and Tables

Lastly we need an assignment table. This should just have the assignment name and a link to the assignment

```
CREATE TABLE zipcode.assignments
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name TINYTEXT NOT NULL,
    URL CHAR(255) NOT NULL
);
CREATE UNIQUE INDEX assignments_id_uindex ON zipcode.assignments (id);
CREATE UNIQUE INDEX assignments_URL_uindex ON zipcode.assignments (URL);
```

We are creating a table similar to the last two, but notice we are making the URL a fixed length `CHAR` field and making that unique. Why?

-

### Schemas and Tables

Last but not least we'll be creating the teacher_meta table. In the meta table, the developers wanna keep track of the number of years a teacher has worked here and the room number of the teacher's office.

```
CREATE TABLE zipcode.teacher_meta
(
    id INTEGER PRIMARY KEY NOT NULL AUTO_INCREMENT,
    room_number TINYINT UNSIGNED,
    years TINYINT UNSIGNED
);
CREATE UNIQUE INDEX teacher_meta_id_uindex ON zipcode.teacher_meta (id);
```

Here we use the `TINYINT` column and make those unsigned

-

### Schemas and Tables

Now we have tables but we have learned that teachers are actually going to have a first name and last name and the devs have said they want these to be two separate fields. Before we continue let's `ALTER` this table.

We are going to `ADD` first name amd last name. Then we will `DROP` name. 

```
ALTER TABLE zipcode.teachers ADD first_name VARCHAR(25) NOT NULL;
ALTER TABLE zipcode.teachers ADD last_name VARCHAR(25) NOT NULL;
ALTER TABLE zipcode.teachers DROP name;
```

-

### One to One
With those tables made, we're gonna have to set this table up with `FOREIGN KEY`s.

The first thing we will be creating is a One to One relationship between teachers and teacher_meta.

```
ALTER TABLE zipcode.teacher_meta ADD teacher_id int NOT NULL;
CREATE UNIQUE INDEX teacher_meta_teacher_id_uindex ON zipcode.teacher_meta (teacher_id);
ALTER TABLE zipcode.teacher_meta
ADD CONSTRAINT teacher_meta_teachers_id_fk
FOREIGN KEY (teacher_id) REFERENCES teachers (id);
```

Here we add teacher_id to the teacher_meta table so that one teacher meta will belong to a teacher. To ensure that each teacher has only one meta, we also make teacher_id on this table unique.
-

### One to Many

Next we'll create the One to Many relationship between teachers and assignments.

One teacher will have many assignments that they've created, and every assignment will belong to one teacher. To get this we will have to add a column called teacher_id on assignment and then mark that as a `FOREIGN KEY`

```
ALTER TABLE zipcode.assignments ADD teacher_id INTEGER NULL;
ALTER TABLE zipcode.assignments
ADD CONSTRAINT teacher___fk
FOREIGN KEY (teacher_id) REFERENCES teachers (id);
```

-

### Many to Many

Next we'll want to create the relationship between students and assignments. In this case, one student can have many assignments, but each of those assignments can also belong to many students. Where should be put the `FOREIGN KEY`?

- In the student table with `assignment_id`
- In the assignment table with `student_id`

-

### Many to Many

Both of those options are wrong. To effectively match up a many to many relationship, we will need a pivot table

a pivot table will have foreign keys to both tables meaning that you can have the same student id match up to multiple assignments and vice verca.

We're also going to create a unique constraint to make sure that the same assignment can't be attached to one student more than once.

-

### Many to Many

```
CREATE TABLE zipcode.assignment_student
(
    assignment_id INTEGER NOT NULL,
    student_id INTEGER NOT NULL,
    CONSTRAINT students__fk FOREIGN KEY (student_id) 
    	REFERENCES students (id),
    CONSTRAINT assignments___fk FOREIGN KEY (assignment_id) 
    	REFERENCES assignments (id)
);
CREATE UNIQUE INDEX assignment_id_student_id_uindex
	ON zipcode.assignment_student (assignment_id, student_id);
```

-
-

# Seeding a Database

-

### Inserts

Our database is set up for our devs to use, but they've asked us to create some mock data for them to demo their app. To do this we'll have to `INSERT` some data.

Let's start by inserting a few teachers.

-

### Inserts

Inserts will generally have 3 parts

- Reference to the table you want to insert into
- The columns you want to fill
- The values to fill those columns with

Let's create a teacher and then that teacher's meta.

-

### Inserts

```
INSERT INTO zipcode.teachers (first_name, last_name, specialty)
    VALUES ('John', 'Smith', 'FRONT END');

```

Here we are creating a new row in the teachers table. The first_name is set to John, the last_name is set to Smith, and the specialty is set to FRONT END. Note that the order of the columns we listed will be the order we list the values. If there is a different number of columns and values, then this will throw an error. Also note that since the id on this table is set to auto increment, it will automatically be filled in with the id of 1. 

-

### Inserts

We now have one teacher in this table and we can add a few more, but instead of running them one by one, we can also just add many at the same time.

```
INSERT INTO zipcode.teachers (first_name, last_name, specialty)
    VALUES ('Tabitha', 'Schultz', 'MIDDLE TIER'),
      ('Jane', 'Herman', 'DATA TIER');
```

-

### Inserts

With the ability to insert data, we can also start populating the other tables

```
INSERT INTO zipcode.teacher_meta (teacher_id, years, room_number)
    VALUES (1, 3, 2),
      (2, 3, 2),
      (3, 10, 1);
```

```
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

```
INSERT INTO zipcode.assignments (name, URL, teacher_id)
  VALUES ('Pokemon Lab', 'https://github.com/Zipcoder/PokemonSqlLab', 3),
  ('Poll App', 'https://github.com/Zipcoder/CR-MacroLabs-Spring-QuickPollApplication', 3),
  ('Sum or Product', 'https://github.com/Zipcoder/ZCW-MicroLabs-JavaFundamentals-SumOrProduct', 2);
```

```
INSERT INTO zipcode.assignment_student (assignment_id, student_id)
    VALUES (1, 1), (1, 2), (1,3), (1,4), (1,5), (1,6), (2, 1);
```

-

### Updates

Updating data is very similar to inserting, but we will use an `UPDATE` clause instead. For this we will need to specify a table we want to update, then a `SET` clause to specify how to change a field or fields.

Our devs inform us that they want a way to increment the number of years each teacher has worked here. Let's see how we would write that update statement

-

### Updates

```
UPDATE zipcode.teacher_meta
SET years = years+1;
```

Here we are able to increment the years by setting years = to whatever it's own value is plus one. This works because SQL will go row by row. In each row, the years variable is set to whatever the value is in that particular row. 

-

### Stored Procedures

Now that we know how to do inserts and updates, it may be useful for us to create a stored procedure for something that will be done regularly. The devs have asked us to put this update statement in a Stored Procedure. 

```
CREATE PROCEDURE zipcode.increment_years_experience ()
BEGIN
  UPDATE zipcode.teacher_meta
    SET years = years+1;
END;
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

<img src="http://img.petshow.cc/ueditor/2016_01/2417092214536265621569945175386650530.jpg">

DBA corgis hope you're `HAVING` a blast!
