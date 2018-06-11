## Object Methods

- `equals()`, `hashCode()`, and `toString()`

-

### equals

- `equals(Object o)` checks equality between this object and Object `o`
- Default implementation returns true only if this object *IS* Object `o`
- Mutable objects should keep default implementation, immutable objects may override
- Overriding `equals` is [dangerous and tricky](http://www.artima.com/lejava/articles/equality.html)

-

### hashCode

- returns a nearly-unique hash (number) representing the object
- used in `HashSet`, `HashMap` and other hashing data structures
- closely related to `equals()` - equal objects must have the same hash code

-

### toString

- Produces a string representation of the object
- default is `m.getClass().getName() + "@" + Integer.toHexString(m.hashCode())`
- automatically called by `System.out` methods
- Override `toString` for a readable way to print your objects.
