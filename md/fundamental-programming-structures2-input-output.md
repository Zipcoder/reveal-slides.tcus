# Fundamental Programming Structures in Java

-
-
##Input and Output

-

###Reading Input

To read console input, you first construct a Scanner that is attached to System.in

```
Scanner in = new Scanner(System.in);

System.out.print("What is your name? ");
String name = in.nextLine();
```
```
String firstName = in.next(); //read a single word
```

-

```
System.out.print("How old are you? ");
int age = in.nextInt();
```

Similarly, the nextDouble method reads the next floating-point number.

-

###Formatting Output

You can print a number x to the console with the statement System.out.print(x). That command will print x with the maximum number of nonzero digits for that type.

```
double x = 10000.0 / 3.0; System.out.print(x);
// prints 3333.3333333333335
```

That is a problem if you want to display, for example, dollars and cents.

-

```
System.out.printf("%8.2f", x); // 3333.33
```
prints x with a field width of 8 characters and a precision of 2 characters.

-

```
System.out.printf("Hello, %s. Next year, you'll be %d", name, age);
```
Each of the format specifiers that start with a % character is replaced with the corresponding argument. The conversion character that ends a format specifier indicates the type of the value to be formatted

[Conversions for printf](http://alvinalexander.com/programming/printf-format-cheat-sheet)

-

In addition, you can specify flags that control the appearance of the formatted output.

```
System.out.printf("%,.2f", 10000.0 / 3.0); // 3,333.33
```
-

You can use the static String.format method to create a formatted string without printing it

```
String message = String.format("Hello, %s. Next year, you'll be %d", name, age);
```

[Flags for printf](http://alvinalexander.com/programming/printf-format-cheat-sheet)

-

###File Input and Output

To read from a file, construct a Scanner object

```
Scanner in = new Scanner(Paths.get("myfile.txt"), "UTF-8");
```
If the file name contains backslashes, remember to escape each of them with an additional backslash: "c:\\mydirectory\\myfile.txt".

-

To write to a file, construct a PrintWriter object.

```
PrintWriter out = new PrintWriter("myfile.txt", "UTF-8");
```

If the file does not exist, it is created.

-

String dir = System.getProperty("user.dir");
