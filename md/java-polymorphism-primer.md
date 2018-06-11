### Polymorphism

-
#### Liskov Substitution Principle

An instance of type T can be replaced by an instance of type S
if
S is a subtype of T.

```Java

public interface GasFueled{}
public interface Electric{}
public class Vehicle{}
  // subtype of Vehicle
public class FordTaurus extends Vehicle implements GasFueled{}
  // subtype of Vehicle
public class TeslaS extends Vehicle implements Electric{}

FordTaurus taurus = new FordTaurus();
Vehicle rental = taurus;
TeslaS snazzy = new TeslaS();
rental = snazzy;              // All of these are legal;
```
-
## Polymorphic Program design

<p class="fragment fade-up">- Most methods rely on provided interfaces, rather than underlying implementations</p>
<p class="fragment fade-up">- Object fields are accessed through getters/setters</>

-
### Things that don't behave polymorphically

<p class="fragment fade-up">- field accesses (eg: `Parent p = new Child(); p.x;`)</p>
<p class="fragment fade-up">- Static methods</p>

-

```Java
//: polymorphism/FieldAccess.java
// Direct field access is determined at compile time.
class Soup {
  public int field = 0;
  public int getField() { return field; }
}
class Stew extends Soup {
  public int field = 1;
  public int getField() { return field; }
  public int getSuperField() { return super.field; }
}
```

-

```Java
public class FieldAccess {
  public static void main(String[] args) {
    Soup soup = new Stew(); // Upcast
    System.out.println("soup.field = " + soup.field +
      ", soup.getField() = " + soup.getField());
    Stew sub = new Stew();
    System.out.println("sub.field = " +
      sub.field + ", sub.getField() = " +
      sub.getField() +
      ", sub.getSuperField() = " +
      sub.getSuperField());
  }
} /* Output:
soup.field = 0, soup.getField() = 1
sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0
*///:~
```
