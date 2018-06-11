### The keyword: `final`

- prevents values and references from changing after initialization
- prevents inheritance of classes
- prevents overriding of methods (but not overloading)
- `static final` produces a compile-time constant

-
### Blank final fields

- A field can be marked final but not initialized until the constructor runs
- All constructors must initialize blank finals, or it is an error.

```
public class Foo{
  final int x;
  public Foo(){
    x = 4;
  }
}
```

-
### final pitfalls

- final object references cannot be changed but the underlying object can  
- blank final fields must be initialized in every constructor
- `final` != `finally` -- these are two different keywords
