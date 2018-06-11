## Inheritance

-
### Terminology

- Subclass - a class that inherits some of its behavior from another class
- Superclass - the class from which a subclass inherits
- Parent/child class - synonyms for superclass and subclass; slightly imprecise
Note: the parent/child/ancestor/descendant metaphor is very common, despite being somewhat misleading.

-
### Class inheritance
- classes can inherit the interfaces of classes and extend their feature set
- this is achieved with the `extends` keyword
- If `B` extends `A` then `B` has all of `A`'s public and protected members and methods
 - package access only within the same package

-
### Inheritance is an "is a" or "is like a" relationship

- An SUV **is a** Vehicle
- A corgi **is a** Dog


-
### Example: SUV

```
public class Vehicle{
  public void start(){...}
}

class SUV extends Vehicle{
  public void drive(){
    start();
    ...
  }
}
```

-
### Example: Corgi
```
class Dog{public void wag(){...}}
public class Corgi extends Dog{
  public static void main(String[] args){
    Corgi bucket = new Corgi();
    bucket.wag();
  }
}
```

[Full example](https://repl.it/FMrL/0)

-
### Upcasting

- Objects can be treated as instances of any superclass in the class hierarchy

```
public class App{
 public static void main(String[] args){
  Dog pembroke = new Corgi();
  pembroke.wag();
  }
}
```
