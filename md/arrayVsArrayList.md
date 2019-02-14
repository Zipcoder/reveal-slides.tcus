# Array and ArrayList
### A tail of friendship, heartache and redemption

-
-

# Differencs between Array and ArrayList

-
##  Resizable

* Array is static in size. Also known as a fixed length data structure. One can not change the length after creating the Array object.

* ArrayList is dynamic in size. Each ArrayList object  has instance variable capacity which indicates the size of the ArrayList. As elements are added to an ArrayList its capacity grows automatically.

-
## Performance

Performance of Array and ArrayList depends on the operation you are performing


-
## Performance

Resize() opertation : Automatic resize of ArrayList will slow down the performance as it will use temporary array to copy elements from the old array to new array.


-
## Performance

add() or get() operation : adding an element or retrieving an element from the array or arraylist object has almost same  performance

-
## Primitives

ArrayList can not contain primitive data types (like int , float , double) it can only contain Objects

Array can contain both primitive data types as well as objects.

-
## Primitives

One misconception that we can store primitives(int,float,double) in ArrayList , but it is not true  

-
## Primitives

Suppose we have ArrayList object,

```
ArrayList  arraylistobject = new ArrayList();
arraylistobject.add(23);  // try to add 23 (primitive)
```

-

JVM through Autoboxing(converting primitives to equivalent objects internally) ensures that only objects are added to the arraylist object.
thus , above step internally works like this :

```
arraylistobject.add( new Integer(23));      
// Converted int primitive to Integer object and added to arraylistobject  
```

-
## Length

Length of the ArrayList is provided by the size() method while each array object has the length variable which returns the length of the array.

for example :

```
Integer arrayobject[] = new Integer[3];
arraylength= arrayobject.length   ;  //uses arrayobject length variable
```

```
ArrayList  arraylistobject = new ArrayList();
arraylistobject.add(12);
arraylistobject.size();   //uses arraylistobject size method
```

-
## Adding elements

 We can insert elements into the arraylist object using the add() method while  in array we insert elements using the assignment operator.

```
Integer addarrayobject[] = new Integer[3];
addarrayobject[0]= new Integer(8)   ;  //new object is added to the array object
```

```
ArrayList  arraylistobject = new ArrayList();
arraylistobject.add(12);
```

-
## Multi-dimensional

 Array can be multi dimensional , while ArrayList is always single dimensional.

 example of multidimensional array:

```
Integer addarrayobject[][] = new Integer[3][2];
addarrayobject[0][0]= new Integer(8)  
```

-
-

# Similarities Between Array and ArrayList

-

1. add and get method : Performance of Array and ArrayList are similar for the add and get operations .Both operations runs in constant time.

2. Duplicate elements : Both array and arraylist can contain duplicate elements.

3. Null Values : Both can store null values and uses index to refer to their elements.

4. Ordered :  Both guarantee ordered  elements. Unlike HashMap and HashSet

-
_

# Difference between Array and ArrayList in Java

|                  | Array               | ArrayList           |
| ---------------- |:-------------------:| -------------------:|
| Resizable        | No                  | Yes                 |
| Primitives       | Yes                 | No                  |
| Iterating values | for, for each       | Iterator , for each |
| Length           | length variable     | size method         |
| Performance      | Fast                | Slow in comparision |
| Multidimensional | Yes                 | No                  |
| Add Elements     | Assignment operator | add method          |
