##Strings

Java does not have a built-in string type. Instead, the standard Java library contains a predefined class called String.

```
String e = ""; // an empty string
String greeting = "Hello";
```

-

###Substrings

You can extract a substring from a larger string with the substring method of the String class.

```
String greeting = "Hello";
String s = greeting.substring(0, 3);
```

The second parameter of substring is the first position that you do not want to copy.

-

###Concatenation

Use + to join (concatenate) two strings.

```
String expletive = "Expletive"; String PG13 = "deleted";
String message = expletive + PG13;
```

-

When you concatenate a string with a value that is not a string, the latter is converted to a string.

```
int age = 13;
String rating = "PG" + age;
```

-

```
String all = String.join(" / ", "S", "M", "L", "XL"); // all is the string "S / M / L / XL"
```

-

###Strings Are Immutable

The String class gives no methods that let you change a character in an existing string.

```
greeting = greeting.substring(0, 3) + "p!";
```

Since you cannot change the individual characters in a Java string, the documentation refers to the objects of the String class as immutable.

-

###Testing Strings for Equality

To test whether two strings are equal, use the equals method.

```
s.equals(t)

"Hello".equals(greeting)

"Hello".equalsIgnoreCase("hello")
```

Do not use the == operator to test whether two strings are equal

-

###Empty and Null Strings

The empty string "" is a string of length 0.

`if (str.length() == 0)`

or

`if (str.equals(""))`

-

However, a String variable can also hold a special value, called null

```
if (str == null)

if (str != null && str.length() != 0)
```
-

###The String API

-

###Reading the Online API Documentation

-

###Building Strings

Using the StringBuilder class allows you to build a string from many small pieces.

```
StringBuilder builder = new StringBuilder();

builder.append(ch); // appends a single character
builder.append(str); // appends a string

String completedString = builder.toString();
```
