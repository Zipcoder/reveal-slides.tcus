# Design Patterns: Structural Patterns





-
-

## What we'll cover

<ul>
<li class="fragment fade-up">What is a Structural Pattern?</li>
	<ul>
		<li class="fragment fade-up">Facade Pattern </li>
		<li class="fragment fade-up">Decorator Pattern </li>
		<li class="fragment fade-up">Adapter Pattern </li>
	</ul>
</ul>










-
-
# Structural Patterns
* eases design by identifying way to realize relationships between entities while reducing code-redundancies
* Some examples:
	* Facade
	* Decorator
	* Adapter















-
-
## Facade Pattern
* provides a simplified interface to a larger body of code, such as a class library.
* prevents client from accessing additional methods that may lead to unexpected user-error.
* _limits_ rather than _extends_ functionality of an encapsulated object.


-
### Encountering Facade Necessity
* Consider the case where a client needs to keep a _collection_ of items, but does only needs to publicly expose 6 methods:
	* `add(ObjectType object): void`
	* `remove(ObjectType object): void`
	* `get(Integer index): ObjectType`
	* `size(): Integer`
	* `foreach(Consumer<ObjectType> consumer): void`

-
### Defining a facade
```java
public class ListFacade<T> {
    private final List<T> list;
    public ListFacade(List<T> list) { this.list = list; }
    public ListFacade() { this(new ArrayList<>()); }

    public boolean add(T object) { return list.add(object); }
    public void remove(T object) { list.remove(object); }
    public T get(Integer index) { return list.get(index);
    }
    public int size() { return list.size(); }
    public void foreach(Consumer<T> consumer) { list.forEach(consumer); }
}
```


-
### Using a Facade
* Client can only access composite `list` field by interfacing with `ListFacade`.
```java
public void demo() {
	ListFacade<String> facade = new ListFacade();
	facade.add("Hello world");
	String firstElement = facade.get(0);
	System.out.println(firstElement);
}
```


















-
-
## Decorator Pattern
* Attach additional responsibilities to an object dynamically
* Provides a flexible alternative to subclassing for extending functionality.



-
### Encountering Decorator Necessity
* Consider the case where an application:
	* has a notion of a `Profile`.
		* `Profile` getters and setters for fields `id`, `name`, and `balance`.
	* has a notion of a `Player`.
		* `Player` getters and setters for fields `id`, `name`, `balance`, and `profile`.
	* this proposed design creates redundancies

-
### Defining Profile
```java
public class Profile {
	// field declaration omitted for brevity
	public Profile(Integer id, String name, Double balance) {
		this.id = id;
		this.name = name;
		this.balance = balance;
	}
	// getter and setter definition omitted for brevity
}
```

-
### Defining Player
```java
public class Player {
	// field declaration omitted for brevity
	public Player(Integer id, String name, Double balance, Profile profile) {
		this.id = id;
		this.name = name;
		this.balance = balance;
		this.profile = profile;
	}
	// getter and setter definition omitted for brevity
}
```

-
### Creating a Decorator
* redundancies from this design are preventable by _decorating_ `Profile` with a `Player`.

```java
public class Player extends Profile {
	private Profile profile;

	public Player(Profile profile) { this.profile = profile; }

	@Override
	public Integer getId() { return this.profile.getId(); }

	@Override
	public String getName() { return this.profile.getName(); }

	@Override
	public Double getId() { return this.profile.getBalance(); }
}
```
* notice that getters and setters in `Player` are inherited, but deferred to composite `Profile` field.

















-
-
## Adapter Pattern
* Converts interface of a class into another interface expected by client.
* Lets classes work together that couldn't otherwise because of incompatible interfaces


-
### Encountering Adapter Necessity
* Consider the case where a client needs to derive a `Date` object from a `LocalDate` object.

-
### Defining an Adapter class
```java
public class TemporalAdapter {
	private Date date;

	public TemporalAdapter(Date date) {
		this.date = date;
	}

	public LocalDate getLocalDate() {
		Instant instant = date.toInstant();
		ZoneId zoneId = ZoneId.of("America/New_York");
		ZonedDateTime zdt = instant.atZone(zoneId);
		LocalDate localDate = zdt.toLocalDate();
		return localDate;
	}
}
```



-
### Using an Adapter class
```java
public void demo() {
	Date date = new Date();
	TemporalAdapter adapter = new TemporalAdapter(date);
	LocalDate localDate = adapter.getLocalDate();
}
```


-
-

<img src="https://github.com/Zipcoder/reveal-slides.tcus/blob/master/img/bunnies/cute-bunnies-tongues-3.jpg?raw=true">

