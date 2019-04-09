# Java Persistance API (JPA)

* Introduction
* Understanding Java Persistence API
* Managing Elementary Entities with JPA

-
-

## Introduction

* What is persistence?
* How do we usually persist data?
* What's wrrong with the way we do?
* How can Java Persistence API help?

### What is Persistence

* Data
* Storage
* Central Point 
* Manipulating data
* Relation databases

### Manipulating Persisted Data Without JPA

* Objects
* Relational databases
* Object-relational mapping
* Rely on external frameworks
* JDBC

### Java DataBase Connectivity

* Java-based data access technolory
* Methods for querying and updating data
* Part of JDK 1.1 on 1997
* JDBC 4.1 is part of JDK 1.7
* Robust
* Low level and verbose

```
public class Book{
	private Long id;
	private String title;
	private String description;
	private Float unitCost;
	private String isbn;
	
	// Constructors, getters & setters
}
```

TODO: finish above example

### What's wrong with JDBC?

* SQL is not Java
* JDBC is a low level API
* SQL is not easy to refactor
* JDBC is verbose
* Hard to read 
* Hard to maintain

### Manipulating Persisted Data With JPA

* Java Persistence API
* Standard
* Object-relational mapping
* Meta-data mapping
* Removes boiler plate code
* (C)reate, (R)ead, (U)pdate, (D)elete
* Object-oriented query language

TODO: Code example with JPA

### Advantages of JPA

* No manual mapping
* No SQL statements
* Interaction through EntityManager interface
* Non intrusive
* Lightweight
* Elegant API
* Powerful

### Summary

* Data is crucial
* We need to stare data
* We need to manipulate data
* Level of abstraction
* Object Relational Mapping
* Simplicity
* Powerful

-
-

## Understanding JPA

* What is a relational database:
* What is an ORM tool?
* What is Java Persistence API?
* What is's not?
* JPA mapping annotations
* API to CRUD and query objects

-

### What is a Database?

* Organized collection of data
* Model our business requirements
* Process information
* Files
* Schema-less
* Relational databases

### What is a Relational Database?

* Collection of tables
* Row
* Columns
* Relationships
* Identity through primary keys

TODO: maybe a database schema diagram

### What is Objet Oriented Programming?

* Objects
* Classes
* Inheritance
* Reference to other classes
* Abstract classes, interfaces, enumerations...
* Encapsulate state and behavior

TODO - maybe a uml diagram that matches the db schema

### What is Object Relational Mapping?

* Bring relational databases and objects together
* Objects have transient state
	* Only available when the JVM is running
	* Some objects need to be persisted
* Relational databases store state
* Mapping between objects and tables
* Java Persistence API

### What is Java Persistence API

* Abstraction above JDBC
* ORM
* Entity Manager
	* (C)reate (R)ead (U)pdate (D)elete operations
	* Java Persistence Query Language
* Transactions
* Life cycle

### What isn't Java Persistence API

* Schema-less databases
* NoSQL
* JSon
* XML
* JPQL != SQL

### A Brief of History of JPA

* ORM solutions have been around for a long time
* Commercial products
* Hibernate in open source
* Successful but not standardized
* JPA 1.0 in 2006 (Java EE 5)
* JPA 2.0 2009 (Java EE 6)
* JPA 2.1 in 2013 (Java EE7)

### JPA Specification

* Specification
* JSR 338
* http://jcp.org/en/jsr/detail?id=338

### JPA Implementations

* Runtime provider
* Implement the JSR 338
* EclipseLink
* Hibernate
* Open JPA

### JPA Packages

| Package                     | Description           |
| --------------------------- | --------------------- |
| javax.persistence           | Core API              |
| javax.persistencecriteria   | Criteria API          |
| javax.persistence.metamodel | Metamodel API         |
| javax.persistence.spi       | SPI for JPA providers |

### Where can we use JPA?

* Java SE
* Java EE

### A JPA Entity

```
@Entity
public class Book {
	@id
	private Long id;
	private String title;
	Private String description;
	
	private Float unitCost;
	private String isbn;
	
	// Constructors, getters & setters
}
```

### Mapping Metadata

TODO: need some text for below

```
@Entity
public class Book {
	@id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	@Column(name = "book_title", nullable = false)
	private String title;
	@Column(length = 2000)
	private String description;
	@Column(name="unit_cost")
	private Float unitCost;
	private String isbn;
	
	// Constructors, getters & setters
}
```

### CRUD Operations

TODO: need some text for below

```
public class Main{
	public static void main(String ...args){
		static EntityManagerFactory emf = Persistence.createEntityManagerFactory("myPU");
		static EnitityManager em = emf.createEntityManager();
		
		public static void main(String ...args){
			Book book = new Book(4044L, "H2G2", "Scifi Book", 12.5f, "1234-5678-5678, 247);
			em.persist(book);
			
			book = em.find(Book.class, 4044L);
			
			TypedQuery<Book> query = em.createQuery("Select b From Book b order by b.title", Book.class);
			query.getResultList();
	}
}
```

### Summary

* What is a relational database
* What is Java Persistence API
* How to develop an entity
* How to manipulate an entity with EntityManager

-
-

## Managing Elementery Entities with JPA

* Managing entities
* Persistence unit
* Mapping entities
	* Annotations
	* XML
* Unit testing

### Managing Entities

* Entity manager
	* EntityManagerFactory in Java SE
	* Injected in JavaEE and Spring
* Persistence context
* Persistence unit

