# String Formatting and Regular Expressions

-
-

## What we'll cover

<ul>
<li class="fragment fade-up">Formatting Strings</li>
<li class="fragment fade-up">Formatting System OutputStream</li>
<li class="fragment fade-up">Formatter Class</li>
<li class="fragment fade-up">Regular expressions</li>
</ul>
-
-

## Formatting Strings

-
### Built-in Formatting Utilities
* `String.format()`
* `System.out.format()`
* `Formatter()`
















-
-
### Formatting: String Formatting
* String formatting converts the value of objects to strings based on formats specified, then inserts them into another string.

-
### Format specifiers
* _Format specifiers_ are _flags_ which notify the compiler to insert values into a `String`.
  * always preceded by a `%`
* Specifier syntax
  * `%[argument_index$][flags][width][.precision]conversion`



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
### Formatting: String Formatting<br>Example
* Formatting `Double` arguments using `%f` specifier
  * `%f` specifies a decimal value
  * `.` precedes positive integer value denoting precision of floating point value. (_decimal precision_)
  * the integer value specifies the _decimal precision_ of the double to formatted.

```java
public void demo() {
  String formattedString = "I've finished %.5f percent of homework";
  Double arg1 = 79.87654321;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I've finished 79.87654 percent of homework
```



-
### Formatting: String Formatting<br>Example
* Specifying precision of `3` decimal places.

```java
public void demo() {
  String formattedString = "I've finished %.3f percent of homework";
  Double arg1 = 79.87654321;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I've finished 79.876 percent of homework
```


-
### Formatting: String Formatting<br>Example
* Specifying precision of `2` decimal places.

```java
public void demo() {
  String formattedString = "I've finished %.2f percent of homework";
  Double arg1 = 79.87654321;
  String outputString = String.format(formattedString, arg1);
  System.out.println(outputString);
}
```

Output
```
I've finished 79.87 percent of homework
```


-
### Formatting: String Formatting<br>Example

```java
public void demo() {
  Integer precision1 = 4;
  Integer precision2 = 5;
  Double valueToFormat = 79.87654321;
  String output1 = getHomeworkDetails(precision1, valueToFormat);
  String output2 = getHomeworkDetails(precision2, valueToFormat);
  System.out.println(output1);
  System.out.println(output2);
}
```

```java
public String getHomeworkDetails(Integer decimalPrecision, Double valueToFormat) {
  String formattedString = new StringBuilder("I've finished %.")
    .append(decimalPrecision)
    .append("f percent of homework")
    .toString();
  return String.format(formattedString, valueToFormat);
}
```

Output
```
I've finished 79.8765 percent of homework
I've finished 79.87654 percent of homework
```






-
### Formatting: String Formatting<br>Example 
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
### Formatting: String Formatting<br>Example 

* Formatting `String` and `Integer` arguments
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
### Formatting: System OutputStream
* OutputStream formatting does not return a `String` value. Rather, it _prints_ a formatted `String` to the console.

-
### Formatting: System OutputStream<br>Example 1
* Formatting `String` arguments using `%s` specifier

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
### Formatting: System OutputStream<br>Example 2
* Formatting `Integer` arguments using `%d` specifier

```java
public void demo() {
  String formattedString = "I am %d years old.";
  Integer arg1 = 25;
  System.out.format(formattedString, arg1);
}
```

Output
```
I am 25 years old.
```




-
### Formatting: System OutputStream<br>Example 3
* Formatting `String` and `Integer` arguments

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
  String formattedString = "Hi, my age is %.1f!";
  Double arg1 = 25.2;

  FileOutputStream outputStream = new FileOutputStream(fileName);
  Formatter formatter = new Formatter(outputStream);
  formatter.format(formattedString, arg1);
  formatter.flush();
}
```

Output: `MyFile.txt` content
```
Hi, my age is 25.2!
```



-
-

##Reminder re: FileOutputStream
When using **FileOutputStream**, you will need to either add a **FileNotFoundException** or use a **try/catch** block





-
-
## Regular expressions
* are a sequence of symbols and characters expressing a string or pattern to be searched for within a longer piece of text.









-
-
### Character classes
| Expression                            | Description                                    |
|---------------------------------------|------------------------------------------------|
| `.`                                   | any character except newline
| `\w`, `\d`, `\s`                      | word / digit / whitespace
| `\W`, `\D`, `\S`                      | not word / not digit / not whitespace
| `[abc]`                               | any of `a`, `b`, or `c`
| `[^abc]`                              | not `a`, not `b`, not `c`
| `[a-g]`                               | character between `a` and `g`
















-
#### Using Character Classes
* Example 1 - Matching all characters

```java
public void demo() {
    String text = "The Quick Brown Fox";
    String patternString = ".";
    Pattern pattern = Pattern.compile(patternString);
    Matcher matcher = pattern.matcher(text);
    for (int i = 0; matcher.find(); i++) {
        System.out.println(new StringBuilder()
                .append("\n-------------------")
                .append("\nValue = " + matcher.group())
                .append("\nMatch Number = " + i)
                .append("\nStarting index = " + matcher.start())
                .append("\nEnding index = " + matcher.end())
                .toString());

    }
}
```

-
### Using Character Classes
* Example 1 output

```
Value = T
Match Number = 0
Starting index = 0
Ending index = 1

-------------------
Value = h
Match Number = 1
Starting index = 1
Ending index = 2

-------------------
Value = e
Match Number = 2
Starting index = 2
Ending index = 3
```


































-
-
### Anchors
| Expression                            | Description                                    |
|---------------------------------------|------------------------------------------------|
| `^abc$`                               | start / end of the string
| `\b`, `\B`                            | word, digit, whitespace





-
#### Using Anchors
* Example 1 - Fetch text from beginning of string

```java
public void demo() {
    String text = "The Quick Brown";
    String patternString = "^The";
    Pattern pattern = Pattern.compile(patternString);
    Matcher matcher = pattern.matcher(text);
    for (int i = 0; matcher.find(); i++) {
        System.out.println(new StringBuilder()
                .append("\n-------------------")
                .append("\nValue = " + matcher.group())
                .append("\nMatch Number = " + i)
                .append("\nStarting index = " + matcher.start())
                .append("\nEnding index = " + matcher.end())
                .toString());
    }
}
```

-
### Using Anchors
* Example 1 output

```
-------------------
Value = The
Match Number = 0
Starting index = 0
Ending index = 3
```







-
#### Using Anchors
* Example 2 - Fetch text from beginning of string

```java
public void demo() {
    String text = "The Quick Brown";
    String patternString = "^Brown";
    Pattern pattern = Pattern.compile(patternString);
    Matcher matcher = pattern.matcher(text);
    for (int i = 0; matcher.find(); i++) {
        System.out.println(new StringBuilder()
                .append("\n-------------------")
                .append("\nValue = " + matcher.group())
                .append("\nMatch Number = " + i)
                .append("\nStarting index = " + matcher.start())
                .append("\nEnding index = " + matcher.end())
                .toString());
    }
}
```

-
### Using Anchors
* Example 2 output - Empty output; no matches

```
```
















-
-
### Escaped characters
| Expression                            | Description                                    |
|---------------------------------------|------------------------------------------------|
| `(abc)`                               | capture group
| `\1`                                  | backreference to group #1
| `(?:abc)`                             | non-capturing group
| `(?=abc)`                             | positive lookahead
| `(?!abc)`                             | negative lookahead





-
#### Using Escaped Characters
* Example 1

```java
public void demo() {
    String text = "The Quick Brown";
    String patternString = "(Brown)";
    Pattern pattern = Pattern.compile(patternString);
    Matcher matcher = pattern.matcher(text);
    for (int i = 0; matcher.find(); i++) {
        System.out.println(new StringBuilder()
                .append("\n-------------------")
                .append("\nValue = " + matcher.group())
                .append("\nMatch Number = " + i)
                .append("\nStarting index = " + matcher.start())
                .append("\nEnding index = " + matcher.end())
                .toString());
    }
}
```

-
### Using Escaped Characters
* Example 1 output

```
-------------------
Value = Brown
Match Number = 0
Starting index = 10
Ending index = 15
```











-
-
### Quantifies and Alternation
| Expression                            | Description                                    |
|---------------------------------------|------------------------------------------------|
| `a*`, `a+`, `a?`                      | 0 or more, 1 or more, 0 or 1
| `a{5}`, `a{2, }`                      | exactly five, two or more
| `a{1,3}`                              | between one & three
| `a+? a{2,}?`                          | match as few as possible






-
#### Using Quantifies and Alternation
* Example 2 - Matching all words

```java
public void demo() {
    String text = "The Quick Brown";
    String patternString = "\\w+";
    Pattern pattern = Pattern.compile(patternString);
    Matcher matcher = pattern.matcher(text);
    for (int i = 0; matcher.find(); i++) {
        System.out.println(new StringBuilder()
                .append("\n-------------------")
                .append("\nValue = " + matcher.group())
                .append("\nMatch Number = " + i)
                .append("\nStarting index = " + matcher.start())
                .append("\nEnding index = " + matcher.end())
                .toString());
    }
}
```

-
#### Using Quantifies and Alternation
* Example 2 output

```
Value = The
Match Number = 0
Starting index = 0
Ending index = 3

-------------------
Value = Quick
Match Number = 1
Starting index = 4
Ending index = 9

-------------------
Value = Brown
Match Number = 2
Starting index = 10
Ending index = 15
```





















-
### More about regex symbols
* `a`, `b`, `c` - match "a", "b", "c" respectively
* `.` - matches any one character
* `*` - match 0 or more occurrences of the last symbol
* `+` - match 1 or more occurrences of the last symbol
* `?` - match 0 or 1 occurrences of the last symbol

-
### More regex examples
* `S117` - matches only the string "S117"
* `dogs?` - matches the strings "dog" or "dogs"
* `fuzzy*` - matches the strings "fuzz", "fuzzy", "fuzzyyyyy" etc.
* `wh?ee+!` - matches "whee!", "weee!", "wheeeee!" [and so on](https://youtu.be/lJ5g6Uq9hwQ?t=4s)

-
### Site for testing regex
* [http://regexr.com/](http://regexr.com/)

-
-
<img src="https://github.com/Zipcoder/reveal-slides.tcus/blob/master/img/bunnies/cutest-bunny-rabbits-23.jpg?raw=true">