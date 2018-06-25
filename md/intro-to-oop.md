# Introduction to Object-Oriented Programming
-
-

### Introduction to Object-Oriented Programming

* Yesterday you had a lecture on procedural programming, which has its place in the world.
* The issue the bigger the problems get, the harder it is to manage the code base.

-


### Introduction to Object-Oriented Programming

* Breaking the problem down into small logical objects
  * each having a single responsibility
* An easier way of managing larger problems
* Provides scalability of solutions over time.

* Allows for greater testability of code.

* Makes program easier to read understand, and debug.

-
-
# Object
* an **Object** is the fundamental unit of OOP
* EVERYTHING is an an Object.
* Sometimes, Java Objects model real-world objects.

* a Car, Truck,
* a Person, an Address, a Printer, a Disk
* a BankAccount, a TaxReturn, a JavaStudent

-
# Object

All **OBJECTS** have

* **Identity** - how that object is distinguished from other objects of the same type. **What's its name?**
* **State** - the value of the internal objects this object contains. **What does it store?**
* **Behavior** - what services or actions this object can perform. **How does it act?**

-

# Classes

* **Class** is template/blueprint for Objects
* You **construct objects from a specific class**
* The **object** you constructed is an **instance of that class** or an OBJECT INSTANCE

* Java has **thousands** of classes in its Standard library
* you've used `System.out` object which is an instance of the `java.io.PrintStream` class

-
# Writing a Class

* IN Java, you basically write/debug/test Java classes all day long.
* When you run a Java program, it creates Objects which do useful (we hope) things
* when it doesn't, you edit a class and run/test it again - forever sometimes

* create **variables** to store State
* write **methods** to describe Behavior
* write **constructors** to construct and initialize an Object and give it an Id
* write **tests** that test the objects and make sure they do what you want

-
# Example Class

BigBossSays

* I need an object which stores a person's name and age.
* I need to get their name and age as a single string.
* And all the persons should be kept separate in different objects.

`file Person.java`
```Java
public class Person {
    // instance variables
    String name;
    int age;

    // constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // instance method
    public String getNameAndAge() {
        return name + " " + age;
    }
}
```

-

# Classes/Objects and the Instance/static difference

* if a **method** is declared **static**
  * you do not need to create an object to call it.
  * e.g. Math.sin(x);

  * if it weren't you'd have to
```
Math math = new Math(); x = math.sin(y);
```

* if a **variable** is declared **static**
  * all objects of that class can use it.
  * `private static long lastSerialNumber`
  * every time an object is constructed, you increment and assign to an instance variable `serialNumber`

-
-
# Encapsulation

*Encapsulation* is simply combining data (variables) and behavior (methods) in one package and hiding the implementation details from the users of the objects.


-
## Objects are services

The objective of **Object oriented programming** is having objects providing services to other objects via messages or calling their public methods.

-

## Single Responsibility

* How an object completes the task that it is asked to do is no one's business but its own.
* The only thing that the asking object cares about is the correct answer; how that answer is generated is irrelevant. Hence, Single Responsibility.

-

## Single Responsibility

* Think about a printer in an office. Do you care how the printer works, or do you only care about the documents you asked it to make?

* It's not your job to print the documents, so you don’t need to or care about how it's done. Only that it gets done.

-
## Single Responsibility

* That's the major premise behind encapsulation,  how an object implements the tasked asked of it, is on a need-to-know basis.

-
-

## Defense

* Encapsulation is also important in defensive programming.  

1. Object methods should only be able to change and interact with instance variables.

2. ObjectA can only interact with ObjectB by calling ObjectB's methods.
  * don't just reach in and change another object's variables. Just Don't.

* This way, you can control the probability of unintended bugs in the future.

-

## Defense

* The value of this is you have the flexibility of changing how a class produces or implements an action, without directly effecting the other classes in the program. It also allows for you to write clear and comprehensive tests for every intended action that an object could be expected to do.

-
-

# Objects
As stated before, all objects have 3 major parts:

* **Identity** - how that object is distinguished from other objects of the same type. **What's its name?**
* **State** - the value of the internal objects this object contains. **What does it store?**
* **Behavior** - what services or actions this object can perform. **How does it act?**

-
## Proper names
* Objects should be named after nouns
  e.g. Car
* Methods should be associated with "action" verbs.
  e.g. start()

* Never write a method named **mightPossiblyDoSomethingUsefulIfYouAskNicely()**
-
-

## Relationships between Classes

* **Dependence** (“Uses-a”) - Think about the objects a class needs to complete its job.

-

## Relationships between Classes

* Objects are containers. Each has its own "state"
* Each employee works slightly different hours each week.
* But they all "punch in" on a single time-clock on the wall each with a different time-card.

How can we model this?

* Think about a scenario in which you have a time-keeping application that has the following objects:
	* Employee
	* TimeCard
	* TimeClock  

-

## Relationships between Classes

* For the TimeCard object to function properly, it needs the TimeClock object to be created and in scope.

* Employee and TimeClock can exist and function without knowledge of TimeCard.

-
-

## Aggregation
* **Aggregation** (“has-a”) - the objects that are contained inside the class

-

### Aggregation

* Programs by nature should avoid complexity, so keep things as simple as possible.

* The objects that you create, and the objects that you use should have a SINGLE-RESPONSIBLITY. They should have one task to do, and do it well.

* Complexity is achieved by creating container classes, which are comprised of simple objects working together to achieve one objective.

-
## Inheritance
* **Inheritance** (“is-a”) - this relationship is polymorphic in nature this relationship talks about what the object is extended from.

* Remember that all objects in java are extended from **Object.class** except for primitives.

-
-

# Objects and Instance Variables
* The **new**  keyword is the magic word that tells the JVM to add a new object to the heap.
* We used **new** back when we created Arrays. That's because an Array object is constructed from the Array **class**
* We commonly construct **objects** with **new**
* Each object has its OWN copies of the 'instance variables'.
```
// maybe Person object has an instance variable 'age'
// joe.age might be 28
// mary.age might be 29
```
-

###Constructors
* **Constructors** - they do not have a TYPE, they just have a name which is the same as the class name.

	* public CustomClass( )
	* A **constructor** has the same name as the class.
	* A class can have more than one **constructor**.
	* A **constructor** has no return value.
	* A **constructor** is always called with the **new** keyword.

-
###Constructors

```
public class Person {
  public Person() {
    /* setup some defaults? */
  }
}

Person joe = new Person();
```
-

###Objects and Object Variables

* Your job as a program is to avoid **NULL** values. OOP is all about Objects talking to Objects by sending messages to each other.

```
Person joe = new Person();
// joe is a reference to a Person Object

Person harlan;          // right now, harlan == null
// harlan is a reference to the Null Object

harlan = new Person();
boolean samePerson = (joe == harlan); // samePerson is false
```
* **Null** means that there is a message being sent to an object that doesn’t exist, and that will lead to a error.

-

##Mutator and Accessor
* A **Mutator** is an action or method that changes the STATE of an object.
* Many times a mutator is required to make objects do useful work.
* Sometimes, mutators don't make sense.
* Mutators can be dangerous events, because if other objects have a dependency on that object, and things are changed unexpectedly or unintentionally, this could lead to false results.

```
joe.setLastName("Smith"); // tends to make sense

LocalDate today = new LocalDate.now()
LocalDate tomorrow = today.plusDays(1);
// today != tomorrow  LocalDate makes sure they are two different objects.
```
-

###Accessors

* **Accessors** - are the privileges we assign to field objects and methods we define in our classes.
    * Public
    * Private
    * Protected
    * Default
* this allows us to open up or hide different things inside an object.
```
public class BankBalance {
	public String owner;
	private int balance;
}
// you can see who owns the account, but not what it's balance is.
```
-
-

##Implicit and Explicit Parameters

* Objects communicate with each other via the Interfaces, which are the public methods defined in their classes.
    * **Implicit** - implied though not plainly expressed.
    * **Explicit** - stated clearly and in detail, leaving no room for confusion or doubt.

-

###Implicit and Explicit Parameters
```
Public Class SpiderMan extends Hero {
    // Constructor
    public SpiderMan( ){…}

    // a method
    public shootWebAtTarget(Target target) { …. }

    // another method
    public selectTargetAndShootWeb( ){
        Target target = new Target( );
        shootWebAtTarget(target);
    }
}
```

-
###Implicit and Explicit Parameters

* Lets look at the method ***shootWebAtTarget( )*** . This method has two parameters and an Implicit parameter which is the SpiderMan object.

* The SpiderMan object is not explicitly called, but is ***implied*** by the compiler when this program is executed.
* The target object is ***explicitly*** stated parameter which has to be stated each time the method is called.

```
SpiderMan toby = new SpiderMan();
Target badguy = new Target();
toby.shootWebAtTarget(badguy);
```
-
-

##Benefits of encapsulation
* Life is full of enough problems, lets avoid them in our programs. We do that by making things as simple as possible. **Simplicity is your friend!**

-
###Benefits of encapsulation

* There is already a possibility of the logic we create being flawed, which would result in errors or unintended results. Encapsulation can’t save us from the issues with our own personal logic, but it can help us avoid issues from unintended side effects of other objects.

-
##Single-Responsiblity
* **Single-Responsiblity**:  when you are defining your objects think about the nature of that object. Lets think about a program that Controls a ATM. Here are the objects that exist in the program:

	* Bank
	* User
	* UserAccount
	* Dollar

-
###Single-Responsiblity

* The **User** object would like to withdraw 100 dollars from his/her account. This should only be allowed if the **UserAccount** associated with that **User** has more than or equal to 100 Dollars in it.

-

###Single-Responsiblity
* If it was up to the **User**, they would take the 100 dollars even if their account did not have the amount in it. It would be awesome if we lived in a world in which everyone could get what they wanted, when they wanted it. Yet, we don't.

* The User should not be allowed to take what ever they wanted, they should only be able to ASK for it.

-

####So who should the User ask for the Money?

* Bank ???
* UserAccount ???
* Dollar ????

-

####So who should the User ask for the Money?

* It makes the most sense for the **User** to ask the **Bank** for the money. Even though the **UserAccount** is dependent on the **User** object, the **User** object is not dependent on the **UserAccount**.

* So since its not dependent on it, it has no reason to know it even exists. In the real world, the **User** would ask the Bank or the Automated Teller Machine at the bank for money.

-

####So who should the User ask for the Money?

*For the **Bank** to decide if they can provide the **User** with the money the **User** is asking for, it is dependent on the **UserAccount** object. The **Bank** must ask the **UserAccount** object if there is enough money in it.

-

####So who should the User ask for the Money?

* A bank can have multiple users, which could become very complicated.
* Keep things simple by making an object to manage user information.
* The bank will ask the **UserAccount** for how much money is available.
* The **UserAccount** will have the amount of **Dollars** stored in it.

-

####So who should the User ask for the Money?

* If this field is public and any object can have access to it, what's to stop the **User** object for simply changing the value of **Dollars** in their account?

-

##Encapsulation

* **Encapsulation** comes in here! The field inside of **UserAccount** called **dollars** will be set to private. Only the **UserAccount** will have access to Mutate that field and change the amount. **SINGLE RESPONSIBILITY**.

-

###Encapsulation

*  When something goes wrong, and only one object has the ability to change or MUTATE the fields, finding out who is responsible and where the error exists becomes extremely fast.

-
-

#Private Methods
* Every object has an Interface
* The Interface is the public methods and fields of an object
* Yet there are times when we need a "private" method in our object, that accomplishes a helper task
* Methods should have a **SINGLE RESPONSIBILITY** (have you heard that term enough?)

-

###Private Methods
```
public String canIBorrowMoney(Person person, Double amount ){
    // First Action
    if ( this.person.equals(person) ) {
       return “no";
    }

    // Second Action
    if ( person.like != true ) {
       return “no;
    }

     // Third Action
    if ( this.amount > amount ) {
       return “Sure thing!";
    } else { // Fourth Action
        return “no”;
    }

}
```

-

###Private Methods

* The problem with this method is we can’t test it, it's doing too much!

	* So how do we fix it?
	* Take each action and make it a private method.
	* We make them private because they only exist to help us answer the one question our interface is making available, *canIBorrowMoney(~)*.

-
###Private Methods

```
private void doIKnowYou(Person person){
    if (!this.person.equals(person)){
        throw new Exception("I don’t know you");
    }
}

private void doILikeYou( Person person){
    if (!person.like){
        throw new Exception("I don’t like you");
    }
}

private void doIHaveEnough( ){
    if ( this.amount < amount ) {
      throw new Exception("I am broke you give me money!");
    }
}
```
-

###Private methods

With those three private methods, we can rewrite the public one using them.

```
public String canIBorrowMoney(Person person, Double amount ){
    doIKnowYou(person);
    doILikeYou(person);
    doIHaveEnough( );
    return “Sure thing, playa”;
}
```

-
###Private Methods

* It doesn’t make sense for these methods to be called by any outside object,  so we make the methods private. We don’t want to allow outside objects to change or mutate any values that could effect our decision, or the way we want to respond to the question being asked of us.

-
-

##The 'final' Key Word
* **Immutable** - unchanging over time or unable to be changed. (think opposite of mutation)
* There are going to be times when we need to guarantee consistency and stability. There are going to be objects in our programs that have values which should NEVER be changed, because it could cause unintended results.

-

##First rule of programming
* **EVERYONE ELSE IS STUPID!!!!** If you don’t explicitly stop someone from doing something, they will eventually do it. Using your code to do it.

-

* Lets say we are creating an application that is dependent on the value of **pi**  equal to 3.14159. The application will pilot a drone around a defined circle in the air.

* It's important for this drone to fly with a high level of precision.

-

* All of the tests we did to validate the drone's precision is based off of the value of **pi** with 5 decimal places.

* All of the objects that help the drone navigate rely on the fact that  **pi** is this precise.

-

* If that value changes during the flight, it could and would lead to a collision or crash.

* So it's important that once in flight that value NEVER changes.

* This is where the **final** keyword comes into play.

-

##final
* **final** is how we define a value as constant or consistent from the start of the application to the end of the application. The value of this variable is set at the start of the application, and cannot be mutated, by any objects at all.

```
private final Float pi = 3.14159;
```
-
-

##Static Fields and Methods

**static** - lacking in movement, action, or change.

We've used `System.out.println()` A LOT. And it's defined:

```
public class System {
   public static final PrintStream out = ...
}

```
This implies that there is ONLY one 'out' object in the entire program.
-

###Static Fields and Methods

* The change part is a little misleading when in the context of Java. The term Static in Java is reference to the field or method in relation to the class it's defined in. When something is defined as static, that means :

	* It does not need an instance of an object to be called or used.
	* The static field or method is a singularity for that class. There is only one of it in existence and it's shared by all objects of that type.

-
###Static Fields and Methods

```
public class Hobbit{
    private static boolean hasRing;

    public void loseRing (){
        Hobbit.hasRing = false;
    }

    public boolean doWeHaveTheRing(){ return Hobbit.hasRing;}
    public Hobbit( ){
        hasRing = true;
    }

}
```

-

* When a class has a static method or field the state is shared by every instance of that class.

```
public class Journey{

    public static void main(String[] args){
        Hobbit frodo = new Hobbit( );
        Hobbit samWise = new Hobbit( );
        // Will Print True
        System.out.println(“Do you have the ring?”
        + frodo.doWeHaveTheRing().toString());
        samWise.loseRing( );
        // Will print False
        System.out.println(“Do you have the ring?”
        + frodo.doWeHaveTheRing().toString());
    }
}
```

-
-

###Static Variable/Fields
* Static variables/fields are very rare.
* Static values violate Object Oriented Principles and **SINGLE RESPONSIBILITY**

-

###Static Constants

* Static Constants are common
* If objects all share the same constant values, there is no need for each instant of that object to have its own value.

```
private final Float pi = 3.14159;
//every instant will have a copy of this value that can never change.

private static final Float pi = 3.14159;
// there will only be one copy shared between any instance created.
```
-
-
##Static Methods
* Static methods are also sometimes referred to and factory methods. They are methods that do not do any direct operations on an object.

```
public class Calculator {
    public static int add (int x, int y) {
        return (x+y);
    }
}
```
-
###Static Methods

* Do we really need to create an instance of the object Calculator for the method **add** to do its job?
* No, we do not. We can just call *Calculator.add(x,y)*;
* Static methods, unlike instance methods are available as soon as the program is started, and are available until the program has completed.

-
-

##The main Method

* The **main** method is the entry point for your application.
* **main** is a reserved word that the JVM looks for to start our programs. Every java program has a main method.

```
static void main(String[] args) { /* do something useful. */ }
```

-
###Main
* Some projects have multiple objects with main methods. The compiler will use the main method of the class file you choose when you decide to run.
* The **main** method is **static** because we need a way to start before any objects are created in memory.

-
-

##Method Parameters
* **call by value** - means that the method gets just the value that caller provides.
* **call by reference** - means that the method gets the location of the variable that the caller provides.
* So what does Java do???

-
###Method Parameters

* Java does manipulate objects by reference, and all object variables are references.

* However, Java doesn't pass method arguments by reference; it passes them by value.

-

###Method Parameters

* The reason for this is that the JVM is always optimizing the Heap. There is no guarantee that the memory block the object is in at one moment, will be the same in the next.

-
-

##Object Construction  
* Objects are containers, the objects we use and create are composed of other objects.
* As stated before, we are in an eternal struggle to avoid null objects.
* Object oriented programming is objects talking to other objects: if we send a message to an object that doesn’t exist, our programs will fail.

-
###Constructors
* This is where constructors come in.
* We use constructors to control the creation of our objects
* We can guarantee that objects are initialized correctly.
* Make sure you are thoughtful about what "defaults" we'd like to see within our objects.

-
####Constructors

* Java does manipulate objects by reference, and all object variables are references. However, Java doesn't pass method arguments by reference; it passes them by value.

```
Person joe = new Person(); // joe is a constructed object.

boss.giveRaise(joe, 100.00);

// inside the boss object we might have
public void giveRaise(Person emp, float raiseAmount) {

  // emp is a reference to the joe object.
  emp.salary = emp.salary + raiseAmount;
}
```

-
##Overloading Methods
**Overloading** is when we have methods that have the

* same name
* return the type
* but take different parameters.

```
public class CarDealer {
  public boolean sell(Car car) {...}
  public boolean sell(Truck truck) {...}
  public boolean sell(Motorcycle moto) {...}
}
```

-

###Overloading

* There are times when we need to construct our objects under different circumstances. We can use overloading to create multiple versions of methods.

-
###Overloading Constructor

```
public class Superman( ){
    private boolean dressedLikeClarkKent;

    // By default we start off in disguise
    public Superman( ) { dressedLikeClarkKent = true; }

    // Allows us to create this object and set the state.
    public Superman(boolean inDisguise){  dressedLikeClarkKent = inDisguise; }
}
```

-

### Default Constructors

* There is always a constructor, even if you do not provide one. When you do not explicitly write a constructor, the compiler will implicitly use the constructor from the class it inherited from. Remember, all objects inherit from *Object.class*

```
public Superman() {}
```

-
-

##Default Field Initialization

* If you don’t set a field explicitly in a constructor, it is automatically set to a default value: numbers to 0, boolean values to false, and object references to null.
* Some people consider it poor programming practice to rely on the defaults. Certainly, it makes it harder for someone to understand your code if fields are being initialized invisibly.

-
-

##Naming conventions

* Life is already hard enough, let's not add complexity where there need not be any. Name things what they are. Sure, we all love the idea of naming every variable after our favorite 80s cartoon characters, but eventually we will forget what String panthero5000 is in reference too.

-
-

##this keyword

* The **this** keyword is used to force an object to call itself. However, it's considered poor form to use it to reference instance variables unless it's essential for clarity.

```
public void setValue(String value){
    /* Here it makes sense because we have two values
       of the same name to explicitly say this.
       If there is not a conflict in naming you should
       let the compiler implicitly associate it.*/

    this.value = value;
}
```
-

* There are times when you might want to call a constructor from inside of another constructor.

```
public class Superman( ){
    private boolean dressedLikeClarkKent;

    //By default we start off in disguise
    public Superman( ) {
	      /* we are calling the constructor and
	      setting the default value we would
	      like set*/
         this.Superman(true);
    }

    // Allows us to create this object and set the state.
    public Superman(boolean inDisguise){  dressedLikeClarkKent = inDisguise; }
}
```
-
-

##Initialization Blocks

* It's important to set the values of fields before we reference them. We use constructors to do that for instance fields, but what about class level or static fields?

-
###Initialization Blocks

* This is where Initialization Blocks come in, they are used to run a set of instructions when the Class is constructed, before any objects are ever created.

```
public Superman ( ){
    private static boolean manOfSteel;

    // When the program starts this code is run before any objects are created
   //  and the constructor is called
    {
        manOfSteel = true;
    }
}
```
-

The static block is ONLY run once when the class is loaded for the first time in the JVM.

the **System** package might do this for the **out** object, so that it's ready to be used right away.

-
-

###Object Destruction and the finalize method

* Once you break the connection between the reference which lives in the stack, and the instance which lives in the heap, there is no way to reconnect with that object. Even though it will not be removed until the next Garbage Collection Cycle, as far as you are concerned, it's gone.

-

###Object Destruction and the finalize method

* There will be times when you want to make sure and validate that an object performs an action before it is removed from GC. For instance , if you are working with sensitive information or connection, you'll need to make sure that things are closed properly. That's where the finalize method comes in.

```
public class Superman( ){
    protected void finalize ( ) throws Throwable {
        // Fly back to the fortress of solitude
    }
}
```

-
-

##Packages
* A **namespace** is a declarative region that provides a scope to the identifiers (the names of types, methods, variables, etc) inside it. Namespaces are used to organize code into logical groups and to prevent name collisions that can occur, especially when your code base includes multiple libraries.

-

###Packages

* What happens when you have two classes called Superman? How do you tell the compiler which Superman you want used when creating new objects? You do that by calling the Package that the class Superman you desire lives in.

```
import dc.action.comics.Superman;
//or
import dc.man.of.steel.Superman;
```

-
-

#Live Demo
