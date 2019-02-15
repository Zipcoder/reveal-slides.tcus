-
-
## Collections vs Maps

Collections hold a series of individual elements.  
Maps store associated pairs of objects (called key-value pairs). The key object is used to look up the value object. Sometimes called dictionaries or associative arrays.  
Both resize automatically (unlike Arrays).



-
Note: `WeakHashMap` uses `WeakReference` objects as keys; can be GC'd if no other references remain
`IdentityHashMap` uses `System.identityHashCode()` and `==` for storage and retrieval


-
## Map types
- TreeMap - Keeps keys in insertion order
- HashMap - Hashes keys for quick access
- LinkedHashMap - Hashes keys, but preserves order with a LinkedList
- WeakHashMap - Objects stored in `WeakHashMap`s can still be garbage collected

-
## Using maps

- Elements are pairs of objects, called keys and values
- keys are used to lookup values
- Check for a specific key with `containsKey(key)`
- Add key-value pairs with `put(key, value)`
- Get values with `get(key)`
- Keys are stored in a Set, retrievable with `keySet()`
