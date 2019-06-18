### Enumeration Classes

-

```
public enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE };
```

The type defined by this declaration is actually a class

You never need to use equals for values of enumerated types. Simply use == to compare them.

-

You can add constructors, methods, and fields to an enumerated type.

```
public enum Size
{
	SMALL("S"), MEDIUM("M"), LARGE("L"), EXTRA_LARGE("XL");

	private String abbreviation;

	private Size(String abbreviation) { this.abbreviation = abbreviation; }
	public String getAbbreviation() { return abbreviation; }
}
```

-

All enumerated types are subclasses of the class Enum. They inherit a number of methods from that class.

`Size.SMALL.toString()` gives "S"

conversely

`Size s = Enum.valueOf(Size.class, "SMALL");`

`Size.MEDIUM.ordinal()` gives a 0-based index (1)... XL is (3)...
