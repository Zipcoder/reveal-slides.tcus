## Collections vs Maps
* Collections hold a series of individual elements.  
* Maps store associated pairs of objects (called key-value pairs).
  * The key object is used to look up the value object.
* Maps are sometimes called _dictionaries_ or _associative arrays_.
* Both, Maps and Collections resize automatically (unlike Arrays).



-
-
### Maps
||||
| --------------- | ------------ |
| HashMap         | Hashes Keys for quick access
| TreeMap         | Keeps keys in insertion order
| EnumMap         | Has key type of specified enum map
| LinkedHashMap   | Hashes keys, preserves order with `LinkedList`
| WeakHashMap     | Has `WeakReference` key-types
| IdentityHashMap | Uses `System.identityHashCode()` and `==` for storage and retrieval
||||


-
## Using maps

- Elements are pairs of objects, called keys and values
- keys are used to lookup values
- Check for a specific key with `containsKey(key)`
- Add key-value pairs with `put(key, value)`
- Get values with `get(key)`
- Keys are stored in a Set, retrievable with `keySet()`
