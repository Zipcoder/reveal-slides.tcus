## 6.1 Interfaces
* **interface** - an interface is not a class but a set of requirements for the classes that want to conform to the interface.
-

  * To create an interface you use the keyword `interface`
  * The interface should contain only method declarations
  * All methods of an interface are implicitly **public**

```
package io.zipcoder;

public interface SmartQuotes {
     String selectRandomQuote();
}
```

-

* The `implements` keyword is used to declare that a class is using an interface

```
public class RandomQuote implements SmartQuotes { ... }

```
-

* Within this class you _must_ implement all methods declared by the interface

```
package io.zipcoder;

public class RandomQuote implements SmartQuotes{

    /**
     *  You must provide a concrete implementation for all the public methods provided
     *  by the interface you are implementing
     */

    public String selectRandomQuote() {
        return "Not So Random Statement";
    }
}
```

-
-

* **Interface Comparable&lt;T&gt;**
  * **Java Comparable** interface is used to order the objects of a user-defined class. 
  * This interface is found in the **java.lang** package and contains only one method `compareTo(Object)`.
  * It provides single sorting sequence only

-

  * Classes provided will most often provide you **compareTo** method to allow them to be sorted, such as **Class String**

```
public int compareTo(String anotherString)
```

-
* For the custom classes that we create, if we intend on giving users the ability to sort them , we need to have our class implement **Comparable<T>** , so that sorting methods know how to evaluate them.

-
```
  public static void main(String[] args){
    Sayian[] sayians = new Sayian[3];

    sayians[0] = new Saiyan("Vegeta", 5000);
    sayians[1] = new Saiyan("Goku", 9000);
    sayians[2] = new Saiyan("Gohan", 3000);

    Arrays.sort(sayians);

    for (Sayian s: sayians)
      System.out.println("Name=" + s.getName() 
          + ", order=" + s.getPowerLevel());

    ...
  }
```
-

    * There is no way for this method to know how to sort these custom classes by default unless this class implements Comparable.
    * Implementing Comparable tells the sort method that these objects can respond to the comparetTo(T t) request.

-

```
public class Saiyan implements Comparable<Saiyan>
{
  ...

  public int compareTo(Saiyan other){
    return Integer.compare(powerLevel, other.powerLevel )
  }
  ...
}
```
-
-
## 6.1.2 Properties of Interface

* Interfaces are not classes, you can never instantiate an interface.
* You can declare a reference variable of type **interface**.
* As long as the class **implements** the interface, you can assign it to that reference variable.

-

If you have this Interface...

```
package io.zipcoder.Marvel

public interface GuardianOfGalaxy {
    Double attackEnemy(Enemy enemy);

    String battleCry();
}
```

-

Then you can create a class that implements that interface...
```
package io.zipcoder.Marvel

public Groot implements GuardianOfGalaxy {

  public Double attackEnemy(Enemy enemy){
     ...
  }

  public String battleCry() {
    return "I am Grooooot!";
  }

}
```
-

* You can create a reference and assign it the Type of an Interface
You DON'T instantiate Interfaces.
* You can, however make that reference variable to any object that **implements** that interface.
* This is a live example of **polymorphism**.

```
package io.zipcoder.Marvel

public class Guardians {

  public assembleGuardians(){
    GuardianOfGalaxy rocket = new GuardianOfGalaxy(); // ERROR
    GuardianOfGalaxy groot = new Groot(); // Works Great
  }
}
```
-
-
## Interface and Abstract Classes

* A class can only extend a single class
  * But there will be times when you would like to apply multiple inheritance to a class.
* This can be accomplished via **implements**.
* Interfaces are polymorphic

-

```
public abstract GuardianOfGalaxy {
    abstract Double attackEnemy(Enemy enemy);

    abstract String battleCry();
}

public abstract Avengers {

  abstact void AvengersAssemble();
}
```

-
```
// an object can only extend one type
public class IronMan extends Avengers , GuardianOfGalaxy{ // Error
  ....
}
```
-
## A Class extends Once, implements Many.

* In Java, an class can only extend from one other class
  * This keeps things simpler
* BUT Classes can implement as many Interfaces as they would like.

-
```
public interface GuardianOfGalaxy {
    Double attackEnemy(Enemy enemy);

    String battleCry();
}

public interface Avengers {

  void AvengersAssemble();
}

public class IronMan implements Avengers, GuardianOfGalaxy{...}

```
-
-

<img src = 'https://c-7npsfqifvt34x24jnhjyx2esbolfsx2edpn.g00.ranker.com/g00/3_c-7x78x78x78.sbolfs.dpn_/c-7NPSFQIFVT34x24iuuqtx3ax2fx2fjnhjy.sbolfs.dpnx2fvtfs_opef_jnhx2f61117x2f2111210247x2fpsjhjobmx2fnjoj-qjht-qipup-v2x3fx78x3d761x26rx3d61x26gnx3dkqhx26gjux3ddspqx26dspqx3dgbdftx26j21d.nbslx3djnbhf_$/$/$/$/$/$'>

-
-



##Static Methods

* For simple implementations, or default implementations, of static methods it's okay to place them inside the interface.
* In older versions of Java this would not be allowed.

-

```
public interface GuardianFlight {

  /**
   * Since this interface only has one simple method
   * there is no reason for us to create a class for this simple
   * of an implementation. We can just provide a static implementation.
   **/

  public static void boardFlight (GuardianOfGalaxy guardian){
    GuardianShip.boardFlight(guardian);
  }
}
```
-
-
##Default Methods

* Using the keyword **Default**, it is possible to supply default implementations for an interface that will be applied to the class.
* Programmers that want to extend the method only need to use the **@Override** annotation

-

```
public interface PowerUpItem {

  /**
   * Initially there is no item equipped to add this to a character
   * you would need to call this method to equip it
   **/
  public void equipPowerUpItem(Item item);

  /**
   * If there is no power item equipped this method could still
   * be called by any class.
   **/
  default Integer usePowerUpItemInAttack(){
    return 0;
  }
}
```
-
-
## 6.1.6 Resolving Default Method Conflicts

* There will be times when implementing two interfaces, they may have methods with the same name with a default implementation.
  * If the superclass provides a concrete implementation, the compiler will pick that execution.
  * If there is not default concrete implementation in the super class, then the compiler will force you to explicitly state it:
    * Person.super.getName();

-
```
public interface Mjolnir {
  default String getNameOfWeapon() {
    return "Mjolnir";
  }
}

public interface CaptainAmericasShield {
  default String getNameOfWeapon() {
    return "Cap's Shield";
  }
}
```
-
```
public class Vision implements Mjolnir, CaptainAmericasShield {

  /**
   * Here we have a class that implements two interfaces with
   * default implementations the compiler has no way of implicitly
   * knowing who should be called so we have to explicitly state it
   **/

  public String getNameOfWeapon(){
    return Mjolnir.super.getNameOfWeapon() 
      + ", " + CaptainAmericasShield.super.getNameOfWeapon();
  }
}
```

-
-

## Interfaces and Callbacks

* A callback method in Java is a method that gets called when an event occurs. Usually, you can implement that by passing an implementation of a certain interface to the system that is responsible for triggering the event E.

-
```
public interface java.awt.event.ActionListener {
  void actionPerformed(ActionEvent e);
}
...

public class ScanForNinjas implements ActionListener {

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("All clear no ninjas at " + e.getWhen() + " time");
    }
}
```

-

Scan for Ninjas every second.
```
public class ScanForEnemies {

    public static void main(String [] args){
        ActionListener scanForNinjas = new ScanForNinjas();

        // Creates a Timer and initializes both the initial delay and between-event delay to delay milliseconds.
        // scanForNinjas gets a call to actionPerformed method every 1000 msecs.
        Timer t = new Timer(1000, scanForNinjas);
        t.start();
        JOptionPane.showMessageDialog(null, "Quit program?");

       System.exit(0);
    }
}
```
-
-
##Lambda Expressions

* lambda expression - is a block of code that you can pass around so it can be executed later, once or multiple times.
* functional programming - a style of building the structure and elements of computer programs—that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data.

-

## Interface Comparator\<T\>

* Type Parameters:`T` - the type of objects that may be compared by this comparator
  * All Known Implementing Classes:[Collator](https://docs.oracle.com/javase/8/docs/api/java/text/Collator.html "class in java.text"), [RuleBasedCollator](https://docs.oracle.com/javase/8/docs/api/java/text/RuleBasedCollator.html "class in java.text")
* Functional Interface:This is a functional interface and can therefore be used as the assignment target for a lambda expression or method reference.
-
```
public class SpeedComparator implements Comparator<Person> {

   public int compare(Person one, Person two){
     return one.speed() - two.speed();
   }
}

...

Arrays.sort(people, new SpeedComparator);
```

-
* There is no reason to create a class just to implement a Comparator
* We can just quickly create a Lambda Expression

-

```
public class Person {

    public int speed;

    public Person(int speed){
        this.speed = speed;    
    }
}
```
-
```
public class Race {

  public void runRace(){
    Person tariq = new Person(1);
    Person husainBolt = new Person(100);

    Person[] people = new Person[]{ tariq, husainBolt};

    // We can create a reference using the interface Comparator
    // Then create the impementation needed for the single method

    Comparator<Person> speedComparator = (racerOne, racerTwo) -> racerOne.speed - racerTwo.speed;

    Arrays.sort(people, speedComparator);
  }

}
```
-
-
_must_ implement all methods within the interface

<img src = 'https://i0.wp.com/theverybesttop10.com/wp-content/uploads/2014/06/Top-10-Baby-Chicks-in-Hats-1.jpg?resize=510%2C410&ssl=1'>


