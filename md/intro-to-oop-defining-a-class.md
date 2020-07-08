# Introduction to Object Oriented Programming

-
-
## What we'll cover

<p class="fragment fade-up">Procedural programming vs. OOP</p>
<p class="fragment fade-up">Classes</p>
<p class="fragment fade-up">Encapsulation</p>
<p class="fragment fade-up">Relationships between classes</p>
<p class="fragment fade-up">Objects and Instance variables</p>


-
-
## What we'll cover (continued)

<p class="fragment fade-up">Single responsibility</p>
<p class="fragment fade-up">Private, Static, Final</p>
<p class="fragment fade-up">Methods</p>
<p class="fragment fade-up">Object Construction</p>
<p class="fragment fade-up">Mutators & Accessors</p>

-
-
# What is procedural programming?
* Procedural (structured) programming consists of designing a set of procedures (algorithms) to solve a problem.
* The procedural paradigm suggests that a programmer will:
	* firstly, identify *which algorithms* to manipulate data
	* secondly, identify *which structures* use which algorithms.

-
## Why do we use procedural programming?
* **Small** problems are easily resolved with a procedural implementation

-
## Why do we not use procedural programming?
* Procedural implementations scale poorly.
* As the program grows in size, its complexity increases.
	* behaviors become tightly-coupled
	* debugging becomes more difficult
	* code-changes and maintenance becomes more difficult
	* testing a single aspect becomes nearly impossible

-
-
## Why do we use OOP?
* **Large** problems can more be more easily scaled using the OOP paradigm.
* OOP allows users to break problem down into small logical objects
* OOP allows users to view code-details within the context of a specific object
* OOP allows users to more easily debug code
* OOP allows greater testability of code


-
## Object Oriented Programming (OOP)
* An object-oriented program is made of objects
* Each object has specific functionalities, which users access via the object's **methods**.
* The OOP paradigm suggests that a programmer will
	* firstly identify *which structures* to manipulate data
	* secondly identify *what algorithms* each structure will use


-
## The 3 aspects of an object
* **Identity** - **What is its location?**
	* How is that object distinguished from other objects of the same type?
* **State** - **What does it store?**
	* What is the value of the internal objects this object contains?
* **Behavior** - **How does it act?**
	* What services or actions this object can perform?
-
-
# Classes
* A **class** is a template, or blueprint from which objects are made
	* it is the cookie-cutter to a cookie
	* it is the _classification_ of an object.

-
## Class naming conventions
* Class names must begin with a letter followed by any combination of letters, digits, and underscores.
  * By convention, class names start with a capital letter.
	* You cannot use a Java _reserved word_ to name a variable or class.
* Whitespace is irrelevant to the Java compiler



-
-
# Encapsulation

*Encapsulation* is simply combining data (variables) and behavior (methods) in one package and hiding the implementation details from the users of the objects.


* Classes *encapsulate* several **data-fields** and behaviors into a single entity.
* **Encapsulation** combines class-members (methods and variables) in a single scope.

-
-
## Instance-Fields
* An instance-field, or instance-variable are representative of the _properties_ or _attributes_ of a `Class`.
* By aggregating the values of an instance's fields, we derive the instance's _state_.

-
# Encapsulation
* "Wraps" several data fields into a single entity

```java
// class signature
public class Person {
	// instance variables (fields)
	private String name;
	private Integer age;
	private Boolean isFemale;

	// constructor
	public Person(String name, Integer age, Boolean isFemale) {
		this.name = name;
		this.age = age;
		this.isFemale = isFemale;
	}
}
```


-
-
# Instance-Methods
* behaviors of an object are made available to users via the object's **methods**
* methods which are invoked on an **object** are **instance-methods**
* method-names should describe the intended behavior of the object
* methods of an object have hidden implementation
* methods describe a "can perform" relationship.


-
## Instance-Methods : declaration

Below is an example of a method declaration and its parts: 
 <img src="https://raw.githubusercontent.com/Zipcoder/reveal-slides.tcus/master/img/method_sig.png" class="diagram" >


-
## Method declaration: Access Modifiers

Java's four choices of access modifier:

- **public**: method can be called from any class.
- **private**: method can only be called from within the same class.
- **protected**: method can only be called from classes in the same package or subclasses. (subclasses will be discusses further later)
- **Default (Package Private) Access**:  method can only be called from classes in the same package. (achieved by simply omitting the modifier)

-
## Method declaration: Optional Specifiers

There are several optional specifiers, these are some of the ones we will cover:

- **static**: Used for class methods.
- **abstract**: Used when not providing a method body.
- **final**: Used when a method is not allowed to be overridden by a subclass. (we'll cover subclasses in depth later)
- **synchronized**: (we'll cover in our Concurrency lesson)


-
## Method declaration: Return Type

Next, we indicate our return type, which tells what type of object is returned by the method. The return type can be an actual Java type, such as **String** or **int**, or **void** which indicates that the method does not return anything.

```Java

int integerReturnMethod() {
    int temp = 7;
    return temp; 
}
int longReturnMethod() {
    int temp = 7F; // DOES NOT COMPILE
    return temp;
}
```
-
## Method declaration: Method Name

The method name is how we identify and call the method at hand. The main convention to observe is that the name cannot begin with a number, and may only contain letters, numbers, $, or _. Also, reserved words, such as "void" are not allowed. 

```Java
public void jump7Feet() { }
public void 7FootJump() { } // DOES NOT COMPILE 
public jump7Feet void() { } // DOES NOT COMPILE 
public void Jump_$() { }
public void() { } // DOES NOT COMPILE
```

-
## Method declaration: Parameter List

The parameter list doesn't *need* to contain any parameters. Multiple parameters are separated by a comma. You identify the parameter's type and the variable name for use in the method. 

Below are some examples of valid and invalid uses:

```Java
public void jump() { }
public void hop { } // DOES NOT COMPILE
public void leap(int a) { }
public void bounce(int a; int b) { } // DOES NOT COMPILE 
public void vault(int a, int b) { }
```

-
## Method declaration: Optional Exception List


Exception lists are optional in a method signature. Here we are showing where it goes if used, but we'll cover them in more depth when we discuss exceptions specifically. Multiple exceptions are separated by commas.

```Java
public void zeroExceptions() { }
public void oneException() throws IllegalArgumentException { } 
public void twoExceptions() throws
IllegalArgumentException, InterruptedException { }
```

-
## Method declaration: Method Body

The last part of a method declaration is the method body (except for abstract methods and interfaces, we'll discuss those in detail later). A method body is simply a code block, indicated by has braces containing zero or more Java statements. 

```Java
public void jump() { }
public void hop; // DOES NOT COMPILE 
public void leap(int a) { int distance = 7; }
```

-
-

## Varargs

A method may use a vararg parameter (variable argu- ment) as if it was an array. However, a vararg parameter **MUST** be the **last** element in a method’s parameter list. This means you can only have one vararg parameter per method.

```Java
public void jump(int... nums) { }
public void leap(int start, int... nums) { }
public void hop(int... nums, int start) { } // DOES NOT COMPILE 
public void bounce(int... start, int... nums) { } // DOES NOT COMPILE
```
-
-
## Instance-Variables (Fields)
* field-values of an object are made available to users via the object's **getters** (accessors)
* variable-values of an object can be manipulated by the user via the object's **setters** (mutators)
* the aggregation of an object's variable's values determines the object's **state**.
* fields describe a "has a" relationship.

-
-
# Access Modifiers

Below are the four access modifiers, in order from most restrictive to least restrictive:

- **private**: Only accessible within the same class
- **default** (package private) access: private and other classes in the same 
- **package protected**: default access and child classes
- **public**: protected and classes in the other packages

-
## Access Modifiers : Private Access

Private access is easy. Only code in the same class can call private methods or access private fields.

-

##Access Modifiers : Default (Package Private) Access

When there is no access modifier, Java uses the default, which is package private access. This means that the member is “private” to classes in the same package. In other words, only classes in the package may access it.

-
## Access Modifiers : Protected Access

Protected access allows everything that default (package private) access allows and more. The protected access modifier adds the ability to access members of a parent class. We'll discuss sublasses and superclasses in depth later. 

-
## Access Modifiers : Public Access

Public access means anyone can access the member from anywhere.

-
## Access Modifiers :Summary

| Can access | If that member is private? | If that member has default (package private) access? | If that member is protected? | If that member is public?
|----------|-------------|------|------|------|
| Member in the same class | Yes |    Yes | Yes | Yes |
| Member in another class in same package | No |    Yes | Yes | Yes |
| Member in a superclass in a different package |  No | No | Yes | Yes|
| Method/field in a non- superclass class in a different package |    No   | No | No | Yes |

-
-
## Static Methods and Fields

Except for the **main()** method, we’ve been looking at instance methods. Static methods don’t require an instance of the class. They are shared among all users of the class. Think of a static member as being a member of the single class object that exists independently of any instances of that class.

-
## Static Methods
- Do we really need to create an instance of the object Calculator for the method add to do its job?
- No, we do not. We can just call Calculator.add(x,y);
- Static methods, unlike instance methods are available as soon as the program is started, and are available until the program has completed.

-
## Static Methods

...which means you can call a static member from its classname. For example, **System.out.println()** or the familiar **main()** method:

```Java
public class Turtle {
    public static int quantity = 0; // static variable 
    public static void main(String[] args) { // static method
        System.out.println(quantity); 
    }
}
```
-
## Static Methods

Just like the JVM basically calls Turtle.main() to run our program, you can do this too, by having a TurtleTester that calls the **main()** method.

```Java
public class TurtleTester {
    public static void main(String[] args) {
        Turtle.main(new String[0]); // call static method 
    }
}
```

-
## Static Methods and Fields


In addition to main() methods, static methods have two main purposes:

- For utility or helper methods that don’t require any object state. (i.e. **Math.round()** )
- For state that is shared by all instances of a class, like a counter. All instances must share the same state. Methods that merely use that state should be static as well.


-
## Static Methods and Fields

Usually, to access a static member, just put the classname before the method or variable and you are done.

```Java
System.out.println(Turtle.quantity); // print the Turtle quantity
Turtle.main(new String[0]); //run our main() method
```

Both of these are nice and easy. There is one rule that is trickier...

-
## Static Methods and Fields

You can call a static method from an instance of the object too! The compiler checks for the type of the reference and uses that instead of the object (sneaky, right?).

```Java
Turtle t = new Turtle();
System.out.println(t.quantity); // t is a Turtle
t = null;
System.out.println(t.quantity); // t is still a Turtle
```

The output of this code is 0, printed twice. The second line sees that **t** is a **Turtle** and **quantity** is a static variable, so it reads that static variable. The fourth line does the same thing. Java doesn’t care that **t** happens to be null. Since we are looking for a static member, it doesn’t matter.

-
## Static Methods and Fields

```Java
Turtle.quantity = 3;
Turtle leo = new Turtle(); 
Turtle mikey = new Turtle(); 
leo.quantity = 5;
mikey.quantity = 4; 
System.out.println(Turtle.quantity);
```

There is only one **quantity** variable since it is static. It is set to 3, then 5, and finally winds up as 4. 

-

# Static vs. Instance

A static member cannot call an instance member. This shouldn’t be a surprise since static doesn’t require any instances of the class to be around.

```Java
public class Static {
    private String name = "My Super-cool Static Class";
    public static void primero() { }
    public static void segundo() { }
    public void tercero() { System.out.println(name); } 
    public static void main(String args[]) {
        primero();
        segundo();
        tercero(); // DOES NOT COMPILE
    }
}
```
-

### Static vs. instance calls

| Type |Calling| Legal? | How?|
|----------|-------------|------|------|------|
| Static method| Another static method or variable| Yes| Using the classname| 
|Static method| An instance method or variable|No||
|Instance method |A static method or variable | Yes |Using the classname or a reference variable |
|Instance method | Another instance method or variable | Yes |Using a reference variable|

-
### Static vs. instance calls

```Java
public class Gorilla {
    public static int count;
    public static void addGorilla() { count++; } 
    public void babyGorilla() { count++; } 
    public void announceBabies() {
        addGorilla();
        babyGorilla(); 
    }
    public static void announceBabiesToEveryone() { 
        addGorilla();
        babyGorilla(); // DOES NOT COMPILE
    }
    public int total;
    public static average = total / count; // DOES NOT COMPILE
}
```

-
### Static Variables

Some static variables are meant to change as the program runs (i.e. counters, meant to increase over time). Similar to instance variables, you can initialize a static variable in its declaration:

```Java
public class Initializers {
    private static int counter = 0; // initialization
}
```

-

### Static Variables

**Constants** are static variables that are meant to never change during the program. Use the **final** modifier to ensure the variable never changes. **static final** constants use a different naming convention than other variables. They use all uppercase letters with underscores between “words.” 

```Java
public class Initializers {
    private static final int NUM_BUCKETS = 45; 
    public static void main(String[] args) {
        NUM_BUCKETS = 5; // DOES NOT COMPILE 
    }
}
```



-

### Static Variables


```Java
private static final ArrayList<String> values = new ArrayList<>(); 
public static void main(String[] args) {
    values.add("changed"); 
}
```
This, however WILL compile, since **values** is a **reference variable**. We are allowed to call methods on reference variables. The compiler checks that we don’t try to reassign the final values to point to a **different** object.

-

### Static Initialization

Static initializers use the **static** keyword to specify they should be run when the class is first used.

```Java
private static final int NUM_SECONDS_PER_HOUR; 

static {
    int numSecondsPerMinute = 60;
    int numMinutesPerHour = 60;
    NUM_SECONDS_PER_HOUR = numSecondsPerMinute * numMinutesPerHour;
}
```
Since the variable **NUM_SECONDS_PER_HOUR** is declared as a constant, but not initialized, it can be initialized in a static block. 



-
-
## Constructors
* a method which creates a new instance of a class (an object)
* describes the initial **state** of an object

-
# Constructor<br>(Default)
```java
public class Person { // class signature
	private String name; // instance variable

	public Person() { // constructor signature
	}
}
```

-
# Constructor<br>(Non-Default)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}
}
```

-
# Multiple Constructors
```java
public class Person { // class signature
	private String myName; // instance variable

	// no-arg (default) constructor
	public Person() { // constructor signature
		this.myName = "Leon"; // setting instance variable
	}

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}
}
```

-
## Assigning initial values<br>From Constructor
```java
public class Person { // class signature
	private String myName; // instance variable
	private Character gender; // instance variable

	// no-arg constructor
	public Person() { // constructor signature
		this.myName = "Leon"; // setting instance variable
		this.myGender = 'M'; // setting instance variable
	}

	public Person(String name, Character gender) { // constructor signature
		this.myName = name; // setting instance variable
		this.myGender = gender; // setting instance variable
	}
}
```

-
## Calling Constructors<br>From Constructors
```java
public class Person { // class signature
	private String myName; // instance variable
	private Character myGender; // instance variable

	// no-arg constructor
	public Person() { // constructor signature
		this("Leon", 'M'); // nested constructor call
	}

	public Person(String name, Character gender) { // constructor signature
		this.myName = name; // setting instance variable
		this.myGender = gender; // setting instance variable
	}
}
```



-
-
# Setters<br>(Mutators)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}

	public void setName(String differentName) { // method signature
		this.myName = differentName; // setting instance variable
	}
}
```

-
# Setters<br>(Mutators)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		setName(name); // BAD DESIGN; method can be overriden
	}

	public void setName(String differentName) { // method signature
		this.myname = differentName; // setting instance variable
	}
}
```

-
# Getters<br>(Accessors)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}

	public void setName(String differentName) { // method signature
		this.myName = differentName; // setting instance variable
	}

	public String getName() {
		return this.myName;
	}
}
```

-
-
<img src="https://raw.githubusercontent.com/Zipcoder/reveal-slides.tcus/master/img/bunnies/cute-bunny77.jpg">
