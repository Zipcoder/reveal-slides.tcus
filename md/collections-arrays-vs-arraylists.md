# Array and ArrayList
### A tale of friendship, heartache and redemption

-
-

## What we'll cover

<p class="fragment fade-up">Differences Between Array and Arraylist</p>
<p class="fragment fade-up">Similarities Between Array and Arraylist</p>
<p class="fragment fade-up">Difference Between Array and Arraylist In Java</p>
<p class="fragment fade-up">Common Array and Arraylist Tasks</p>

-
-
# Differences between Array and ArrayList


-
##  Resizable

* Array is static in size. Also known as a fixed length data structure.
  * One can not change the `length` after creating the Array object.
* `ArrayList` is dynamic in `size`.
  * Each `ArrayList` object has instance variable capacity which indicates the size of the `ArrayList`.
  * As elements are added to an `ArrayList` its capacity grows automatically.

-
## Performance
* Performance of Array and `ArrayList` depends on the operation you are performing


-
## Performance
* `Resize()` operation : Automatic resize of `ArrayList` will slow down the performance as it will use temporary array to copy elements from the old array to new array.


-
## Performance

`add()` or `get()` operation : adding an element or retrieving an element from the Array or `ArrayList` object has almost same performance

-
## Primitives
* One misconception is that we can store primitives (`int`, `float`, `double`) in `ArrayList`, but it is not true  

-
## Primitives
* `ArrayList` can only contain _non-primitve_ data types (like `Integer`, `Float`, `Double`)
* `ArrayList` can not contain _primitive_ data types (like `int`, `float`, `double`)
* Array can contain both _primitive_ as well as _non-primitive_ (`Object`s) data types.



-
## Primitives
* Suppose we have `ArrayList` object,

```java
ArrayList<Integer>  arraylistobject = new ArrayList();
arraylistobject.add(23);  // try to add 23 (primitive)
```

-
## ArrayList Autoboxing
JVM through Autoboxing (converting primitives to equivalent objects internally) ensures that only objects are added to the `arraylistobject`.
thus , above step internally works like this :

```java
arraylistobject.add(new Integer(23));      
// Converted int primitive to Integer object and added to arraylistobject  
```

-
## Length
* The number of elements in an `ArrayList` is provided by the `size()` method
* The number of elements in an array is provided by the `length` variable.

for example:
```java
Integer arrayobject[] = new Integer[3];
arraylength= arrayobject.length; //uses arrayobject length variable
```

```java
ArrayList<Integer>  arraylistobject = new ArrayList();
arraylistobject.add(12);
arraylistobject.size(); //uses arraylistobject size method
```



-
## Adding elements
 * We can insert elements into the `arraylistobject` using the `add()` method.
 * We can insert elements into the array using the assignment operator.

for example:
```java
Integer addarrayobject[] = new Integer[3];
addarrayobject[0]=new Integer(8); //new object is added to the array object
```

```java
ArrayList<Integer>  arraylistobject = new ArrayList();
arraylistobject.add(12);
```

-
## Multi-dimensional
* Array can be _multi-dimensional_, while ArrayList is always _single dimensional_.

for example:

```java
Integer addarrayobject[][] = new Integer[3][2];
addarrayobject[0][0]= new Integer(8);
```

-
-
# Similarities Between Array and ArrayList

-
1. `add` and `get` method: Performance of Array and `ArrayList` are similar for the `add` and `get` operations.
2. Duplicate elements: Both Array and `ArrayList` can contain duplicate elements.
  * Unlike `Set`s and `Map`s (no duplicate keys)
3. Null Values: Both can store `null` values and uses index to refer to their elements.
4. Ordered:  Both guarantee ordered elements.
  * Unlike `HashMap` and `HashSet`

-
-
# Difference between Array and ArrayList in Java

-

|                  | Array               | ArrayList           |
| ---------------- |:-------------------:| -------------------:|
| Resizable        | No                  | Yes                 |
| Primitives       | Yes                 | No                  |
| Iterating values | for, for each       | Iterator,for each   |
| Length           | length variable     | size method         |
| Performance      | Fast                | Slow in comparision |
| Multidimensional | Yes                 | No                  |
| Add Elements     | Assignment operator | add method          |



-
-
# Common Array and Arraylist Tasks

-
## Declaration and Instantiation
Array:
```java
String[] villains = new String[5];
```

ArrayList:
```java
ArrayList<String> heroes = new ArrayList<String>();
```

-
## Add Items

Array:
```java
villians[0] = "Dark Pheonix";
villians[1] = "Deadshod";
villians[2] = "CatWoman";
villians[3] = "Green Goblin";
villians[4] = "Poison Ivy";
```

ArrayList:
```java
heros.add("Hellboy");
heros.add("Storm");
heros.add("Spawn");
heros.add("Silver Surfer");
heros.add("Mr. Fantastic");
```

-
## Access an Item
Array:
```java
villians[3];
```

ArrayList:
```java
heroes.get(3);
```

-
## Change an Item
Array:
```java
vilians[4] = "Apocalypse";
```

ArrayList:
```java
heroes.set(4, "The Tick");
```

-
## Remove an Item

Array:
* There is no way to remove and item from an array.
* You will need to duplicate values into a new array sans the value to remove.

ArrayList:
```java
heroes.remove(0);
```

-
## Clear an Item
Array:
* There is no way to clear an array.
* You will need to create a new array

ArrayList:
```java
heroes.clear();
```


-
## Length of Items
Array:
```java
villians.length;
```

ArrayList:
```java
heroes.size();
```
