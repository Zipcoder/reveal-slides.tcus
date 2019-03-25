# Dependency Injection


-
## Dependencies

- Classes often **depend** on other classes for some functionality
- Dependencies can be provided (injected) from other sources as well

-
### Manual Dependency Management

```Java
public class Instructor{
	
	Cohort students;
	RosterManager rm;
	
	public Instructor(){
		rm = RosterManager.getInstance();
		students = new Cohort(rm.getRoster("April 2017"));
	}
	
}
```

-
### Constructor Injection Example

```Java
public class Instructor{
	
	Cohort students; // We could also inject this
	RosterManager rm;
	
	public Instructor(RosterManager rm){
		this.rm = rm;
		students = new Cohort(rm.getRoster("April 2017"));
	}
}
```

-
### Setter Injection Example

```Java
public class Instructor{
	
	Cohort students;
	RosterManager rm;
	
	public void setStudents(Cohort students){
		this.students = students;
	}
	
	public void setRosterManager(RosterManager rm){ this.rm = rm; }
}
```


-
### Benefits

- Easier to mock dependencies for tests
- Classes are loosely coupled
  - Allows independent development of components
- Dependencies can be selected at runtime
