
-
## Creational Patterns: Singleton
* Problem:
	* Ensure a class only has one instance and provide a global access point to it.
* Different Archetypes:
	* Eager initialization
	* Lazy initialization
	* Enum singleton
 	* Bill Pugh singleton


-
### Eager Initialization
* **Solution:** the instance of Singleton Class is created at the time of class loading
* **Consequence:** instance is created even if client application might not be using it.

```java
public class EagerSingleton {    
    private static final EagerSingleton instance = new EagerSingleton();

    private EagerSingleton(){}

    public static EagerSingleton getInstance(){
        return instance;
    }
}
```

-
### Lazy Initialization
* **Solution:** creates the instance in the global access method.
* **Consequence:** a bit verbose

```java
public class LazySingleton {
    private static LazySingleton instance;

    private LazySingleton(){}

    public static LazySingleton getInstance(){
        if(instance == null){
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

-
### Bill Pugh Singleton
* **Solution:**
	* inner class is only loaded upon invokation of `getInstance`.
	* doesn't require synchronization
* **Consequence:**

```java
public class BillPughSingleton {

    private BillPughSingleton(){}

    private static class SingletonHelper{
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance(){
        return SingletonHelper.INSTANCE;
    }
}
```

-
### Enum Singleton
* **Solution:** Java ensures that any enum value is instantiated only once.
* **Consequence:** does not allow lazy initialization.

```java
public enum EnumSingleton {
    INSTANCE;
}
```

-
### Usage
* Notice the two different syntaxes for referencing a singleton object.

```java
public class Main {
	public static void main(String[] args) {
		PersonFactory personFactory = PersonFactory.getInstance();
		PersonWarehouse personWarehouse = PersonWarehouse.INSTANCE;

		Person person1 = personFactory.createPerson();
		Person person2 = personFactory.createPerson();
		Person person3 = personFactory.createPerson();

		personWarehouse.add(person1, person2, person3);
	}
}
```




-
-


![Image of Puppies](https://i.ytimg.com/vi/zdcAbMwO6Zs/maxresdefault.jpg)


-
-
## Creational Patterns: Factory
* Defers some part of object construction to another class
* [Factory Method]((https://github.com/Zipcoder/TC-LectureDemo-DesignPrinciples/blob/master/src/main/java/io/zipcoder/design_demo/stage4/Main.java))
	* A method which is responsible for instantiating and returning an object.
* [Factory Pattern](https://github.com/Zipcoder/TC-LectureDemo-DesignPrinciples/blob/master/src/main/java/io/zipcoder/design_demo/stage6/Main.java)
	* A system which includes a class solely responsible for creation of another.
* Abstract Factory Class Pattern
	* Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

-
-


![Image of Puppies](https://i.ytimg.com/vi/bxvoEiCas5Y/maxresdefault.jpg)


-
-
## Creational Patterns: Builder
* Separate the construction of a complex object from its representation so that the same construction process can create different representations

* Builder pattern was introduced to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.

-

There are three major issues with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.
1. Too Many arguments to pass from client program to the Factory class that can be error prone because most of the time, the type of arguments are same and from client side its hard to maintain the order of the argument.
2. Some of the parameters might be optional but in Factory pattern, we are forced to send all the parameters and optional parameters need to send as NULL.
3. If the object is heavy and its creation is complex, then all that complexity will be part of Factory classes that is confusing.
