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
* `String.format`
* String formatting is

-
### Formatting: String Formatting<br>Example 1
* Formatting `String` arguments

```java
public void demo() {

}
```




-
### Formatting: String Formatting<br>Example 2
* Formatting `Number` arguments

```java
public void demo() {
}
```













-
-
### Formatting: System Outstream
* `System.out.format`
* Outstream formatting is used



-
### Formatting: System Outstream<br>Example 2
* Outstream formatting `String` arguments

```java
public void demo() {

}
```


-
### Formatting: System Outstream<br>Example 2
* Outstream formatting `Number` arguments

```java
public void demo() {

}
```






















-
-
### Formatting: Formatter<br>Example 1
* `Formatter`
* Formatter object is used to



-
### Formatting: Formatter<br>Example 1
* Formatting `String` arguments

```java
public void demo() {

}
```


-
### Formatting: Formatter<br>Example 2
* Outstream formatting `Number` arguments

```java
public void demo() {
    Formatter f=new Formatter();
    f.format(Locale.FRANCE,"%.5f", -1325.789);
    System.out.println(f);
}
```

Output
```
-1325,78900
```


-
### Formatting: Formatter<br>Example 3
* Outstream formatting `Number` arguments

```java
public void demo() {
    Formatter f=new Formatter();
    f.format(Locale.Canada,"%.5f", -1325.789);
    System.out.println(f);
}
```

Output
```
-1325.78900
```


-
### Formatting: Formatter<br>Example 4
* Assumes default `Locale` of System

```java
public void demo() {
    Formatter f=new Formatter();
    f.format("%.5f", -1325.789);
    System.out.println(f);
}
```

Output
```
-1325.78900
```





-
### Formatting: Formatter<br>Example 5
* Abstracting `Locale` numeric formattings

```java
public static void main(String[] args) {
    String franceDecimalValue = getDecimalValueAs(Locale.FRANCE, 3, 152.65);
    String canadaDecimalValue = getDecimalValueAs(Locale.CANADA, 3, 152.65);
    System.out.println(franceDecimalValue);
    System.out.println(canadaDecimalValue);
}
```

```java
public String getDecimalValueAs(Locale locale, Integer decimalPrecision, Number valueToFormat) {
    Formatter f=new Formatter();
    f.format(locale,"%." + decimalPrecision + "f", valueToFormat);
    String output = f.toString();
    return output;
}
```

Output
```
152,650
152.650
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
