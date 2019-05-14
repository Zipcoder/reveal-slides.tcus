#Introduction to Objects

-
-
## What we'll cover
<p class="fragment fade-up">Abstraction</p>
<p class="fragment fade-up">5 Pillars of OOP</p>
<p class="fragment fade-up">Objects</p>
<p class="fragment fade-up">Classes</p>


-
-
#You down with OOP?

Object Oriented Programming is a way of abstracting computer programs to look and model like real world items or objects.

-
# Abstraction
Earlier programming languages focused on abstracting the machine. What OOP strives to do is abstract the real world into models.

It allows the programmer to represent elements in the **problem space** or **domain**.

-
# Abstraction

OOP allows you to express a problem in terms of the problem, and not on the computer where the problem will run.

"Computer Science is not the science of computers, its the science of solving problems with the computer"

-
-
# 1 of 5 Pillars of OOP

Everything is an Object! The first step in solving a problem is breaking that problem down in to individual objects.

-

# 2 of 5 Pillars of OOP

Objects talk to Objects.

A program is a collection of objects sending and responding to messages between each other.

-

# 3 of 5 Pillars of OOP

Objects are containers. Each object has its own unshared block of memory that consist of other objects.

-

# 4 of 5 Pillars of OOP

Every Object has a **TYPE**

-

# 5 of 5 Pillars of OOP

All objects of a particular type can recieve the same messages.

-
-

#What is an object?

Every object consist of the 3 basic things :

* Identity
* State
* Behavior

-
-
#What is a Class?

A class is a template or a blueprint that explains and details what an object is, and how it should be created.

-
-
#What is a Interface?

A interface is a representation of messages that can be sent to an object.

-
-

#An Object provides services

* Objects are service providers or **workers**.
* Your goal is to find an object that provides the service you need, or make it yourself.
* Programming starts with decomposing the problem into its logically smallest parts.

-
-

#Object Creation and Lifetime

* C++ uses static memory allocation, the developer has full control over memory.
* Java uses Dynamic memory allocation , when a new program is started up in run time the **JVM** creates a memory **HEAP** for objects to be created in.
* Garbage Collection automatically removes unreferenced objects from the heap.

-
-

#EVERYTHING IS AN OBJECT DAMN IT

Haven't heard that before??

Java is an object oriented language and you need to switch your mindset to completely understand and master it.

-
-

#References

You never actually deal with objects in Java, you get only have access to them via their references.

```
String s; // a reference is created but not an object
s = new String("Hello World"); 
// the refernece is now associated with an object in the heap.
```

-
-
#5 places to store data

1. **Registers** - they exist inside the processor (very limited)
2. **Stack** - RAM and the fastest but limited by needing to know the exact life time of allocated storage. Object References live here not objects.
3. **Heap** - General purpose memory all of the java objects live here. You place an object in the heap by using the "**new**" command.
4. **Constant storage** - Constant **static** values created by the program, which can not change during runtime.
5. **Non-Ram** - Files , Streams , Databases

-
-
#Primitives

Everything is an object unless its a primitive... which is still an object, but ummmmm not really??? 

Conceptually everything is always an object. Yet there is an actual Class in Java called **Object.class**. Which everything except primitives inherit from.

```
String \\ Test results say Object is the Father.
int \\ Test results say Object is NOT the Father.
```

-

#Primitive Types

* boolean
* char 16 bits
* byte 8 bits
* short 16 bits
* int 32 bits
* long 64 bits
* float 32 bits
* double 64 bits
* void

-
-
#Arrays in Java

* Java makes you initialize arrays to b an exact size.
* Arrays do not hold objects / they hold the references to the objects.

-
-
#Garbage Collection

You never need to explicitly delete an object in JAVA. In fact its almost impossible to do so, since you do not have direct access to memory. To remove an object from memory, you must remove every reference to that object. GC will remove any object that is not actively reference. 

Once an object reference is broken, you have no access to that object again.

-
#Garbage Collection

```
// Here we have created a new object and reference
String s = new String("Hello world"); // #StringObject001

// Now we set the reference to another new Object

s = new String("Well Hello again"); // #StringObject002

// #StringObject001 has no reference and will be removed next time
// GC runs.

```
-
-

#UML DEMO

