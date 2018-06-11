# JDBC and Spring-JDBC

-
-
## What is JDBC?

- Java Database Connectivity
- Standard API for relational database access
- Part of Java SE, found in `java.sql` and `javax.sql`

-
## JDBC Boilerplate example

```Java
private static final String SQL_INSERT_CAR =
	"insert into car (make, model, year) values (?, ?, ?)";
@Autowired
private DataSource datasource;

public void addCar(Car car){
  try(Connection conn = dataSource.getConnection(),
      PreparedStatement stmt = conn.prepareStatement(SQL_INSERT_CAR)){
    stmt.setString(1, car.getMake());
    stmt.setString(2, car.getModel());
    stmt.setString(3, car.getYear());
  }catch(SQLException e){
    //handle any of a thousand different issues..?
  }
}
```

-
-
## Spring JDBC templates

- Uses the "Template Method" design pattern to simplify access
- Wraps caught `SQLException` with a more descriptive Spring runtime exception

-
## JDBC Template Example

```Java

private static final String SQL_INSERT_CAR =
  "insert into car (make, model, year) values (?, ?, ?)";

@Autowired
JdbcTemplate jdbcTemplate;
//some examples will use the superinterface JdbcOperations

public void addCar(Car car){
	jdbcTemplate.update(SQL_INSERT_CAR,
  				car.getMake(),
  				car.getModel(),
  				car.getYear());
}

```


-
## Named parameter queries

- Use `NamedParameterJDBCTemplate`
- Allows using keywords instead of indexed wildcards
- Accepts a map of columns to values
- Allows batch updating with an array of maps
- Easier to read the queries than `JdbcTemplate`

-
## Named Parameter example

```Java
@Autowired
NamedParameterJdbcTemplate namedParamTemplate;

private static final String INSERT_CAR =
	"INSERT INTO car (make, model, year) " +
	"VALUES (:make, :model, :year);";

public void addCar(Car car){
  Map<String, Object> paramMap = new HashMap<>();
  paramMap.put("make", car.getMake());
  paramMap.put("model", car.getModel());
  paramMap.put("year", car.getyear());
  namedParamTemplate.update(INSERT_CAR, paramMap);
```

-
-
## ResultSet

- Represents a set of rows returned by a query
- Provides getters for many datatypes
- getters available for column name or index
- Default implementation only allows `next()` to traverse rows

-
## RowMapper

- Interface to take 1 row from a result set and produce an object
- implements `mapRow(ResultSet rs, int rowNum)`
- `mapRow(...)` Called once per row returned
- Shouldn't call `ResultSet.next()`

-
## RowMapper Example

```Java
public Car findOne(long id) {
	return jdbcTemplate.queryForObject(
					SELECT_CAR_BY_ID,
					new CarRowMapper(),
					id);
}

private static final class CarRowMapper implements RowMapper<Car> {
  public Car mapRow(ResultSet rs, int rowNum) throws SQLException {
    return new Car(
      rs.getString("make"),
      rs.getString("model"),
      rs.getString("year"));
  }
}
```

-
## ResultSetExtractor

- Used to process all rows in a result set
- often used when multiple rows form one object
- Good for building collections from query results

-
## ResultSetExtractor Example

```Java
public class CarPriceMapExtractor
		implements ResultSetExtractor<Map<String, BigDecimal>>{
	@Override
	public Map<String, BigDecimal> extractData(ResultSet rs)
			throws SQLException, DataAccessException{
		Map<String, BigDecimal> res = new Hashmap<>();
		while(rs.next()){
			res.put(rs.getString(1), rs.getBigDecimal(2));
		}
		return res;
	}		
}
```

-
## Using ResultSetExtractor

```Java

String priceQuery = "SELECT package, price "
	+ "from auto_prices as p, cars as c "
	+ "WHERE c.id = p.car_id "
	+ "AND make = ? "
	+ "AND model = ? "
	+ "AND year = ?;";

public Map<String, BigDecimal> getPriceMap(Car c){
	return jdbcTemplate.query(priceQuery, new CarPriceMapExtractor(),
		car.getMake(),
		car.getModel(),
		car.getYear());
}
```

-
## When do I use which one?

- 1 row -> 1 object -- RowMapper
- x rows -> y objects (x != y) -- ResultSetExtractor

-
-
## Security Concerns

-
SQL Injection

- One of the most common DB vulnerabilities
- Potential danger any time you take user input


-
Resources

- Spring Security
- [Open Web Application Security Project - OWASP](https://www.owasp.org/index.php/Main_Page)
