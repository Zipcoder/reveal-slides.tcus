## Reflection

-
### Reflection basics

- `java.lang.reflect` package contains classes for examining code at runtime
- `Constructor`, `Method`, and `Field` classes represent constructors, methods, and fields
- `Class` objects provide `getFields()`, `getMethods()` and `getConstructors()` methods

-
## The Class Object

- One for every class in your program
- Used for creating objects
- Contains info about classes

-
### Class Object Creation

- Created by the ClassLoader
- Created on first reference of a static member of the class

-
### Getting a Class object

- `Object.getClass()` on an instance of the desired class
- Class literal (eg: `Object.class` or `String.class`)
- `Class.forName(String className)` eg: `Class.forName("java.lang.String")`


-
### Checking the class of an object

- `instanceof` keyword - requires name of class at compile time
- `isInstance()` method - allows dynamic checking
- class object equality (`==` or `.equals()`)

-
### Example

```Java
String str = "Hello";
str instanceof String; //true
Object.class.isInstance(str); // true
str.getClass == String.class // true
str.getClass == Object.class // false
str.getClass.equals(Object.class) //false
```


-
### Useful Class object methods

- `getName()`, `getSimpleName()`, `getCanonicalName()`
- `newInstance()`
- `cast()`, `getSuperclass()`


-
### Resources

- [`Class` class documentation](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)
