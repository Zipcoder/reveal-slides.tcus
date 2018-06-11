# JPA

Java Persistence API

-
## JPA Specification

- Part of Java EE spec
- Contained in the `javax.persistence` package
- Implemented by many other libraries

-
## Hibernate

- Popular implementation of JPA
- Handles mapping Entity classes to DB tables
- Included in `spring-data-jpa` dependency by default
- Finds entities with entity scanning


-
## ORM (definition)

- Object Relational Mapping
- The practice of/tool for relating objects to DB rows

-
-
## Spring repositories

-
## Repositories

- Spring's DAO objects
- The way to use your data
- Part of spring framework
- Used to provide JPA implementation
- Automatically implemented by Spring Boot (during startup)
- Extend JPARepository or CRUDRepository

-
## Existing repository types

- `Repository` - parent marker interface
- `CRUDRepository` - repository defines CRUD operations
- `JpaRepository` - extended from `CRUDRepository`, defines 18 methods and can contain custom methods


-
Method names parsed and automatically implemented

`<verb><subject?>By<predicate>()`  
eg: `findCarByYear()` or `countDistictPersonByLastName()`  


-
### Verbs

- `get`, `read`, `find` -- queries and returns objects (synonyms)
- `count` -- returns a count of matching objects

-
### Subject

- Optional
- Produces distinct query if the word `distinct` is present

-
### Predicate

- references a property
- may specify [comparison operations](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#_supported_query_keywords) (`equals` is implied)
- `ignoresCase`/`ignoringCase` usable for case-insensitive comparison
- predicate properties map to arguments

-
Repositories provide indirect JPA access -- we can still directly access using `@persistencecontext`/`@entitymanager` (later)

-
-
## JPA Annotations

- @Entity
- @ID
- @PersistenceContext
- `@OneToMany`, `@ManyToOne`, `@OneToOne`, `@ManyToMany`
- [Full list](https://docs.oracle.com/javaee/7/api/index.html?javax/persistence/package-summary.html)

-
@Entity

- Domain objects are "entities"
- Entities are managed by the EntityManager
- Table name defaults to class name
  - use `@Table(name = "...")` to change this

-
@ID

- Sets the field (usually `int` or `long`) as the primary key
- Usually paired with `@GeneratedValue`

-
`@OneToMany`, `@ManyToOne`, `@OneToOne`, `@ManyToMany`

Indicate relationships between entities eg:

```
@Entity
public class Room{
...
@OneToMany
private List<Window> windows;
```

-
-
More about Repository methods

-
Access Sub-properties in predicates

- `findBy<PropertyName><SubpropertyName>`
- eg: `findByAddressState()`
- Optional underscore separator: `findByAddress_ZipCode()`  

-
Specify custom queries

- `@Query` annotation on method signature
- Queries written in JPQL
- Serves as a hint for the Spring-generated implementation

-
Custom query example

```Java
@Query("select c from Customer c where c.emailAddress = ?1")
Customer findByEmailAddress(EmailAddress email);
```

-
Custom implementations (example to follow)

- Create an interface to define the desired method(s)
- Implement custom JPA method in `<RepositoryName>Impl`, implementing interface
- Extend the same inteface in `<RepositoryName>`

-
Custom JPA example

```Java
interface ClassicCarUpdater {
  int updateClassics();
}

public class CarRepositoryImpl implements ClassicCarUpdater {
  @PersistenceContext
  private EntityManager em;

  public int updateClassics() {
    String update =
      "UPDATE Cars car SET car.isClassic = true " +
      "WHERE car.isClassic = false " +
      "AND car.id IN (" +
      "SELECT c from Cars c WHERE c.year < 1992)";
    return em.createQuery(update).executeUpdate();
  }
}
```

-
Custom JPA example (repository)

```Java
public interface CarRepository extends
        JPARepository<Car, Integer>,
        ClassicCarUpdater {
    ...
    }
```

-
-
Configuration hints:

- Show hibernate generated queries with `spring.jpa.show-sql=true` in `application.properties`

-
@PersistenceContext and @PersistenceUnit

- @PersistenceUnit injects an EntityManagerFactory
- @PersistenceContext injects an EntityManager



