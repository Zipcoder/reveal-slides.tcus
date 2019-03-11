# Regular expressions

-
-
## Formatting Strings

-
### Built-in Formatting Utilities
- `String.format()`
- `System.out.format()`
- `Formatter()`
















-
-
### Formatting: String Formatting
* String formatting converts the value of objects to strings based on formats specified, then inserts them into another string.

-
### Format specifiers
* _Format specifiers_ are _flags_ which notify the compiler to insert values into a `String`.
  * always preceded by a `%`


-
### Formatting: String Formatting<br>Example 1
* Formatting `String` arguments using `%s` specifier
  * `%s` specifies a `String` value.

```java
public void demo() {
  String formattedString = "Hey! [ %s ] is my name!";
  String arg1 = "John";
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
Hey! [ John ] is my name!
```


-
### Formatting: String Formatting<br>Example 2
* Formatting `Integer` arguments using `%d` specifier
  * `%d` specifies an integer value

```java
public void demo() {
  String formattedString = "I am %d years old.";
  Integer arg1 = 25;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I am 25 years old.
```



-
### Formatting: String Formatting<br>Example 3.0
* Formatting `Double` arguments using `%f` specifier
  * `%f` specifies a decimal value
  * `.` precedes positive integer value denoting precision of floating point value. (_decimal precision_)
  * the integer value specifies the _decimal precision_ of the double to formatted.

```java
public void demo() {
  String formattedString = "I have finished %.5f percent of my homework.";
  Double arg1 = 79.87654321;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I have finished 79.87654 percent of my homework.
```



-
### Formatting: String Formatting<br>Example 3.1
* Specifying precision of `3` decimal places.

```java
public void demo() {
  String formattedString = "I have finished %.3f percent of my homework.";
  Double arg1 = 79.87654321;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I have finished 79.876 percent of my homework.
```


-
### Formatting: String Formatting<br>Example 3.2
* Specifying precision of `2` decimal places.

```java
public void demo() {
  String formattedString = "I have finished %.2f percent of my homework.";
  Double arg1 = 79.87654321;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I have finished 79.87 percent of my homework.
```


-
### Formatting: String Formatting<br>Example 3.2

```java
public void demo() {
  Integer precision1 = 4;
  Integer precision2 = 5;
  Double valueToFormat = 79.87654321;
  String output = getHomeworkDetails(precision1, valueToFormat)
  String output = getHomeworkDetails(precision2, valueToFormat)
  System.out.println(output);
}
```

```java
public String getHomeworkDetails(Integer decimalPrecision, Double valueToFormat) {
  String formattedString = new StringBuilder()
    .append("I have finished %.")
    .append(decimalPrecision)
    .append("f percent of my homework.")
    .toString();
  String outputString = String.format(formattedString, arg1);
  return outputString;
}
```

Output
```
I have finished 79.8765 percent of my homework.
I have finished 79.87654 percent of my homework.
```






-
### Formatting: String Formatting<br>Example 4
* Formatting `Character` arguments using `%c` specifier
  * `%c` specifies a character value

```java
public void demo() {
  String formattedString = "My first initial is %c.";
  Character arg1 = 'J';
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
My first initial is J.
```




-
### Formatting: String Formatting<br>Example 5
* Formatting `String` and `Number` arguments
  * Arguments are entered in the order they are specified

```java
public void demo() {
  String formattedString = "Hey! My name is %s. I am %d years old.";
  String arg1 = "John";
  Integer arg2 = 25;
  String outputString = String.format(formattedString, arg1, arg2);
  System.out.println(outputString);
}
```

Output
```
Hey! My name is John. I am 25 years old.
```















-
-
### Formatting: System Outstream<br>
* Outstream formatting does not return a `String` value. Rather, it _prints_ a formatted `String` to the console.

-
### Formatting: System Outstream<br>Example 1
* Formatting `String` arguments using `%s` specifier
  * `%s` specifies a `String` value.

```java
public void demo() {
  String formattedString = "Hey! [ %s ] is my name!";
  String arg1 = "John";
  System.out.format(formattedString, arg1);
}
```

Output
```
Hey! [ John ] is my name!
```


-
### Formatting: System Outstream<br>Example 2
* Formatting `Number` arguments using `%d` specifier
  * `%d` specifies a non-decimal integer

```java
public void demo() {
  String formattedString = "I am %d years old.";
  Integer arg1 = 25;
  String outputString = System.out.format(formattedString, arg1);
}
```

Output
```
I am 25 years old.
```




-
### Formatting: System Outstream<br>Example 3
* Formatting `String` and `Number` arguments
  * Arguments are entered in the order they are specified

```java
public void demo() {
  String formattedString = "Hey! My name is %s. I am %d years old.";
  String arg1 = "John";
  Integer arg2 = 25;
  System.out.format(formattedString, arg1, arg2);
}
```

Output
```
Hey! My name is John. I am 25 years old.
```























-
-
### Formatting: Formatter Class
* used to format and output data to a specific destination, such as a string or a file output stream.



-
### Formatting: Formatter<br>Example 1
* Formatting `String` arguments


```java
public void demo() {
  String fileName = "MyFile.txt";
  String formattedString = "Hi, my name is %s!";
  String arg1 = "John";

  FileOutputStream outputStream = new FileOutputStream(fileName);
  Formatter formatter = new Formatter(outputStream);
  formatter.format(formattedString, arg1);
  formatter.flush();
}
```

Output: `MyFile.txt` content
```
Hi, my name is John!
```


-
### Formatting: Formatter<br>Example 2
* Formatting `Integer` arguments


```java
public void demo() {
  String fileName = "MyFile.txt";
  String formattedString = "Hi, my age is %d!";
  Integer arg1 = 25;

  FileOutputStream outputStream = new FileOutputStream(fileName);
  Formatter formatter = new Formatter(outputStream);
  formatter.format(formattedString, arg1);
  formatter.flush();
}
```

Output: `MyFile.txt` content
```
Hi, my age is 25!
```


-
### Formatting: Formatter<br>Example 3
* Formatting `Double` arguments


```java
public void demo() {
  String fileName = "MyFile.txt";
  String formattedString = "Hi, my age is %d!";
  Integer arg1 = 25;

  FileOutputStream outputStream = new FileOutputStream(fileName);
  Formatter formatter = new Formatter(outputStream);
  formatter.format(formattedString, arg1);
  formatter.flush();
}
```

Output: `MyFile.txt` content
```
Hi, my age is 25!
```










-
-
## Regular expressions

-
### Basic symbols
- `a`, `b`, `c` - match "a", "b", "c" respectively (all numbers & letters)
- `.` - matches any one character
- `*` - match 0 or more occurrences of the last symbol
- `+` - match 1 or more occurrences of the last symbol
- `?` - match 0 or 1 occurrences of the last symbol

-
### Examples
- `S117` - matches only the string "S117"
- `dogs?` - matches the strings "dog" or "dogs"
- `fuzzy*` - matches the strings "fuzz", "fuzzy", "fuzzyyyyy" etc.
- `wh?ee+!` - matches "whee!", "weee!", "wheeeee!" [and so on](https://youtu.be/lJ5g6Uq9hwQ?t=4s)

-
### Regex sites
- [regexr](http://regexr.com/) - test regular expressions












-
### Format parameters

- first argument: The format string with format specifiers
- all other arguments: values to fill into the format string

-
### Format specifier syntax

`%[argument_index$][flags][width][.precision]conversion`

[Example](https://repl.it/DrIh/2)

-


![Format Conversions](img/FormatConversions.png)

-
### Pitfalls
* Some specifiers may behave in unexpected ways. eg: `%b` will produce "true" for non-null values of any other data type

-
-
### Useful Resources

- [Apache Commons StringUtils](http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/StringUtils.html)
- [Java Strings tutorial](https://docs.oracle.com/javase/tutorial/java/data/strings.html)
- [CSV RFC (spec)](https://tools.ietf.org/html/rfc4180) -- just read #1-7 in section 2
