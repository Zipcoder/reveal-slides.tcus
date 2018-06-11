### All classes inherit from the Object class

- `Object` is the immediate superclass if `extends` is omitted

-
## Extending Object class
 - valid but redundant
 - ...unless you've defined an `Object` class of your own (please don't)

-
### Example: Explicit "extends Object"


Both the same:

```
class Thing {}
```

```
class Thing extends Object {}
```
