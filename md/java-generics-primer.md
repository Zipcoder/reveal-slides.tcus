### Generics

-
### Generics

- Generics allow type flexibility in code.
- Allows for behavior independent of type. (like a LIFO "stack")  
- Generic type indicated with `<T>` or `<K, V>` usually  
- Compiler replaces generic types with given type (eg: `ArrayList<String>()`)

-

### Generics - a FIFO Queue

- queue: a data structure that allows for FIRST IN, FIRST OUT.
- this is a **thought experiment**, just **imagine** having this thing...
- Queue s = new Queue(); give you a "queue for waiting objects"
- s.append(object1); s.append(object2);
- when you s.next(); you get object1,
- if you do it again - s.next(); you get object2
- if you do a s.next(); again, you maybe get an error or a the null object

- imagine a Queue of Persons at DMV. "standing in line"

-

### Hmm.

- BUT how to we make this generic enough to use for anything: Cars at a CarWash
- We don't want to have PersonQueue, CarQueue, etc.
- think of all the copied code, duplicates, a different class for every kind of object...
- if you're thinking **UGH**, what's that **code smell**?
  - you're starting to understand

-
### Queue

this is an imaginary example...
```Java
// Pretend Queue has a add() method and a next() method.
Queue personQueue = new Queue<Person>(); // this queue is for persons

personQueue.add(joe); // joe starts standing in line
currentPerson = personQueue.next();  // "Next!" person being serviced

Queue carWashQueue = new Queue<Vehicle>(); // this queue tracks cars to be washed
carWashQueue.add(teslaS);
```


-

##Terms

- Parameterized Type
- Generic Type
- Type Inference

-

## Examples of generic Types

- List (ArrayList, LinkedList, etc)
```Java
List carWashQueue = new ArrayList<Vehicle>();
```
- Map (HashMap, TreeMap, etc)
```Java
Map carsWeSell = new HashMap<String,Vehicle>();
carsWeSell.put("Tesla S", teslaS)
```

-
### Primitive types cannot be type parameters

Example:

```Java
// This will not compile!!!!

public class Main{
  public static void main(String[] args){
    ArrayList<int> primitiveArrayList = new ArrayList<>();
  }
}
```

-
### The solution? Wrapper classes!

```Java

public class Main{
  public static void main(String[] args){
    ArrayList<Integer> pg = new ArrayList<>();
  }
}
```

-
### Primitives and autoboxing

```Java
public static void main(String[] args){
  int x, y;
  x = 5;
  ArrayList<Integer> box = new ArrayList<>();
  box.add(x);
  y = box.get(0);
  System.out.println(y);
```
