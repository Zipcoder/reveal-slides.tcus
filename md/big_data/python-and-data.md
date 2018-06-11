# Dealing with Data the Python Way

-
-

## Python Basics

-

### Basic code

Python is a loosely typed scripting language. Code runs top down and is interpreted sequencially.

```python
import json # this line will run first

data = {"message": "Hello World!"} # this line will run second
message = json.loads(data).get("message") # this will run third
print(message) # this will run last
```

-

### Basic code 

Python, like Java, comes with a set of libraries available right from the start. These libraries are considered part of the Python Standard Library.

You can use a library with the `import` keyword. 

```python
import os # functions that interact with the operating system

print(os.getcwd()) # get current working directory and print it
```

-

### Data Types

There are many data types in python but these are the most common to see

* Booleans are either True or False.
* Numbers can be integers (1 and 2), floats (1.1 and 1.2), and complex numbers.
* Strings are sequences of Unicode characters
* Lists are ordered sequences of values.
* Tuples are ordered, immutable sequences of values.
* Sets are unordered bags of unique values.
* Dictionaries are unordered bags of key-value pairs.

-

### Booleans

Booleans are expressions that evaluate to `True` or `False`. In python this is one of the four primitive types

```python
print(True)           # true
print(False)          # false
print(True and False) # false
print(True or False)  # true
print(bool(0))        # false

a = 1
print(a is 1)         # true
```

-

### Numbers

Numbers are decalared and their types are assumed by how they are written in the code. Integers and Floats are both primitive types in python

```python
x = 1   # x is an integer
y = 1.0 # y is a floating point number
z = 1j  # z is a complex number
```

-

### Numbers

You may use basic mathematical operations with numbers

```python
1 + 1   # 2
2.0 + 2 # 4.0
2 + 1j  # (2+1j)

3/2     # 1.5
3//2    # 1
```

-

### Strings

Strings are a sequence of characters. In python strings are a primitive type. They can be denoted with either double or single quotes. `'Hello'` and `"Hello"` are the same string. 

```python
x = "Hello World"
print(x)    # prints Hello World
print(x[0]) # prints H
```

-

### Lists

Lists are an ordered group of things. Most comparable to arrays except you can store multiple different types into the same list

```python
l = ["a", "b", "c"]
print(l[0]) # a

l2 = ["a", 2, 6.5, l]

print( l2[1] )     # 2
print( l2[-1] )    # ["a", "b", "c"]
print( l2[-1][0] ) # "a"
print( l2[4] )     # out of bounds error
```

-

### Lists

Lists can be sliced into smallers lists using slice notation

```
letters = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']

letters[0:4]         # ['a', 'b', 'c', 'd']
letters[8:10]        # ['i', 'j']
letters[:6]          # ['a', 'b', 'c', 'd', 'e', 'f']
letters[18:]         # ['s', 't', 'u', 'v', 'w', 'x', 'y', 'z']
letters[-3:]         # ['x', 'y', 'z']
letters[20:-1]       # ['u', 'v', 'w', 'x', 'y']
letters[5000:]       # []
letters[-5000:50000] # whole alphabet
```

-

### Tuples

Tuples are an ordered group of immutable objects.

```python
t = ("a",2,3.0) # assign a tuple
t = "a",2,3.0   # the parans are optional

print(t[0])     # a

a, b, c = t     # tuple unpacking: a is "a", b is 2 c is 3.0
print(c)        # 3.0

t[0] = "b"      # results in an error because you cannot 
                # change an immutable item
```

-

### Sets

Sets are like lists except they have a uniqueness quality and they are unordered.

```python
s = {1,1,2,3} # results in a set with 1 2 and 3 in it
print(s)      # {1,2,3}

s[0]          # will result in error because there are no indexes

list(s)[0]    # will result in some number coming out 
              # since a set isn't ordered it can be random
```

-

### Dictionaries

Dictionaries are an unordered bag of key value pairs.

Keys and values can be any type, but some types may behave strnagely when used in cojunction

```python
d = {"a": 1, "b": 2, 3, "c"}

print(d["a"])        # 1
print(d[3])          # c

d[3] = "some value"  # d[3] would print "some value" now
d[3.0] = "new value" # we are using a float 3.0 as a new key
print(d[3])          # prints "new value"
```

-
-

# Control Flow

-

### Conditionals and Loops

In python we will want to test properties and control the flow of our code. We do this using conditional statements and loops. 

-

### if statements

An if statement tests a boolean expression for truth, and if it is true it will run a block of code underneath it. A block is started using a `:` character and a new line with at least 2 spaces. Once you write a line that is unindented, the if statement is over. 

```python
a = 5
if a == 5: #notice the colon starting the block
    print("a is equal to five")
print("I am out of the if statement now")
```

-

### if statements

Sometimes we'll want to do something when the first if statement isn't true. If this is the case we can use elif and else to extend our if statements.

```python
a = 1
if a is 3: # for primitive types we may use is safely
    print("a is equal to 3")
elif a is 2:
    print("a is equal to 2")
else:
    print("a is not 2 or 3")
```

-

### Aside - Tabs vs Spaces

Instead of indenting your code with 4 spaces, you may indent them with a tab character. As long as you are consistent with how you indent, your code will run just fine, but you may not mix tabs and spaces within the same block of code for indenting. 

It is recommended that you use spaces over tabs because tabs are not uniform between machines and moving code from one machine to another may not work as expected. This problem is fairly uncommon and ultimately this is just a matter of preference.

-

### Loops

There are two main types of loops in python. While loops and For loops.

-

### While Loops

A while loop will check a condition and run a block of code as long as that condition is true

```python
i = 0
while i is not 10:
    print(i)
    i+=1
print("I am outside the loop now")
```

-

### For Loops

A for loop takes an interative item and moves through it. The syntax is `for {item} in {list of items}:` followed by a block of code. within the for loop, the variable decalared first will be assigned to an item in the iterative and will be usable. 

```python
list_of_numbers = [1,2,3]
for number in list_of_numbers:
    print(number)
print("I am out of the for loop now")
```

-

### For Loops

You can also loop over a list of tuples using unpacking

```python
for item1, item2 in [("a", "x"), ("b", "y"), ("c", "z")]:
    print(item1, " ", item2)
```

-

### For Loops

You may loop over a dictionary. If you do this, the for loop will assign the key to item. 

```python
for key in {"a": 1, "b": 2, "c": 3}:
    print(key) # results in a, b, c being printed
```

You can unpack a dictionary into a dict_items object using the items method on a dictionary. This object can be iterated over just like a list of tuples.

```python
d = {"a": 1, "b": 2, "c": 3}
for key, value in d.items():
    print(key, " : ", value)
```

-

### Aside - Scope

Python does not create new scopes for loops, so any variable defined in the loop, including any variable in the loop signature will be available outside of the loop. 

```python
for item in [1,2,3]:
    item2 = "another item"
print(item) # will print 3
print(item2) # will print "another item"
```

-

### Loop controls

Within a loop block you may use one of the following keywords to help with the flow

* break - exit a loop and continue the program
* continue - move to the next iteration of the loop without completing the current block
* pass - used when a statement is required syntactically but you want to continue the program without doing anything

-

### Installing a library

You can install a library outside the standard library using the package manager pip. 

To install some package type in the command `pip install {package}` to your terminal. Once that is done you may use that package by simply using the `import` command in your python.

```bash
$ pip install pymysql
```
```python
import pymysql

connection = pymysql.connect(...)
```

-
-

# Data

-

### Strucutures

When dealing with data, you will often have to analyze the structure of your data. Often times you will be dealing with lists, dictionaries, and tuples in some mix. 

```python
# List of people represented as tuples
people = [("Wilhem", "Alcivar", 23), ("Leon", "Hunter", 24)]

for first_name, last_name, age in people:
    pass

# List of people represented as dictionaries
people = [{"first_name" : "Wilhem", "last_name": "Alcivar", "age": 23}, {"first_name" : "Leon", "last_name": "Hunter", "age": 24}]

# this would be the equivilent of the above loop
for person in people:
    first_name, last_name, age = person.values()
    pass
```

-

### Connecting to a database

Connecting to a databse in python is fairly similar to connecting to a databse with an IDE or Data Studio tool

To connect to a database, import the proper driver and supply the credentials to that driver object. 

-

### Connecting to a database

Python has no such thing as an interface so there is no enforecement for any driver to adhere to any specific set of methods, but Python as an organization writes up specifications for commonly written drivers. Most major drivers will adhere to their specifications with little or no variance.

[Database API specifications can be found here ](https://www.python.org/dev/peps/pep-0249/)

-

### Connecting to a databse

To connect to a MySQL database we'll use the `pymysql` driver. You can install this with the command `pip install pymsql`

```python
# Connecting to a local database instance
import pymysql

HOST = 'localhost'
USER = 'username'
PASSWORD = 'password'
DATABASE = 'db_name'
PORT = 3306

connection = pymysql.connect(host=HOST, user=USER,
    password=PASSWORD, db=DATABASE, port=PORT)
```

You can read more about connection objects in python's connection specifications

-

### Working with connections

Now that we have a connection object, we want to start querying data. Lets say we have a database on localhost and it has a table with users in it. The columns are `first_name VARCHAR(40)`, `last_name VARCHAR(40)`, and `age TINYINT UNSIGNED`

We may select some data using a cursor object. The specifications for the cursor object can be found with the Database API specification. 

-

### Working with connections

```python
with connection.cursor() as cursor: # create a cursor object
    # Read a single record
    sql = """
        SELECT `first_name`, `last_name`, `age` 
        FROM `users` WHERE `age` > %d
    """ # %d is a placeholder

    # inject an item into query placeholder
    cursor.execute(sql, (20,))

    # cursor will have results that can be iterated over
    # use fetchone to grab the next item or fetchall
    # for every item in the cursor.
    result = cursor.fetchone() 

    print(result)
```

<small>[credit](http://pymysql.readthedocs.io/en/latest/user/examples.html)</small>

-

### Working with connections

When inserting data programatically, the data wont change after the statement executes. 

To persist changes, you must commit those changes to the connection. 

```python
connection.commit()
```

This is the default behavior and it is recommended that you do not alter this.
-

### Aside - Security Concerns

If you noticed in our SELECT statement we decided to use the `%d` placeholder and then hardcoded in the integer 20. 

This is to avoid a SQL Injection attack. Most of the times when you programatically run a query, you will be dealing with user input. If this is the case you could inject that input with simple string concatenation, but SQL will run whatever string you put in regardless of the result.

<strong>NEVER TRUST USER INPUT, NOT EVEN YOUR OWN</strong>

-

User inputs: `'; DROP TABLE users; #`

```python
SQL = "SELECT * FROM users WHERE username = '"\
    + user_input + "' AND active=1;"
# This will result in a bad query

# SELECT * FROM users WHERE username = '';
# DROP TABLE users; # AND active=1;

# This will destroy your data if you commit this

SQL = "SELECT * FROM users WHERE username = %s AND active=1;"
connection.cursor().execute(SQL, user_input)
# The resulting query will be safe to execute

# SELECT * FROM users 
# WHERE username = '\'DROP TABLE users; #' AND active=1;
```

-
-

# Generating reports

-

### Working with files

When working with data, often you'll want to generate a report to display data in a user friendly format or to transfer over a service. 

One of the most common formats people will request is CSV or Comma Separated Values. To generate this, we'll have to collect our data and write that to a file. We can use Python's [open](https://docs.python.org/3/library/functions.html#open) method to get a [file object](https://docs.python.org/3/glossary.html#term-file-object) to work with 

-

### Working with files

There are some general security concerns with files that python takes care of for us. Normally file writing involves opening a file, inserting data, committing it, and closing that file. At any point you must ensure the file closes if you are done with it to avoid data leakage.

If a program closes without closing a file, your OS may not be able to detect that and it may leave your file exposed to edits from non-privledged users.

-

### Working with files

In python, using a with block, we can open a file, interact with it, and when the block ends the file will close no matter how we exited the block.

```python
with open('file.txt', 'w') as file: # opening a file called file.txt
    file.write('some content\n') # write content to the file
# file is closed here

with open('file.txt', 'r') as file: # reopen the file in read mode
    print(file.read()) # prints some content
# file closes again
```
-

### Creating the report

With those notes, we may start actually generating our report safely and securely.

Lets say we have a connection and we are have a sql statement in a variable called `sql_statement`

```python
with connection.cursor() as cursor: # create a cursor object
    # inject an item into query placeholder
    cursor.execute(sql_statement)
    data = cursor.fetchall()


```
-

### Creating the report

Now we may take that data and write it to a file


```python
with open('users.csv', 'w') as user_file:
    for first_name, last_name, age in data:
        user_file.write(first_name)
        user_file.write(',')
        user_file.write(last_name)
        user_file.write(',')
        user_file.write(age)
        user_file.write('\n')
```    

The resulting file would write a CSV file that could be opened in common programs like Microsoft Excel

-

<img src="https://static.boredpanda.com/blog/wp-content/uuuploads/cute-baby-animals-2/cute-baby-animals-2-2.jpg">