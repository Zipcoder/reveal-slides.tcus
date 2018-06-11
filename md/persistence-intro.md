# Persistence Basics

-
## What does persistence mean

- Data needed by a program to operate sticks around ("Persists") through restarts/re-deploy
- Important information needed across multiple runs is stored somewhere

-
## persistence options

- Run the program; copy & paste the output
- Never restart the program
- Read/write to file
- Use a database

-
### Example: Bank Accounts

- Deploying new software shouldn't reset accounts
- Bugs or crashes shouldn't impact customer balances
- Multiple applications may need access to account data

-
### Separation of Concerns

Your application shouldn't be responsible for storing data, only knowing where to find it and what to do with it.

Databases live/run for a long time; programs may change daily/hourly

-
-
### Database types

-
#### Relational Databases

- Mysql, sqlite, Postgresql
- Store data in rows in tables
- Most (but not all) use SQL or similar
- Tables have a fixed schema (set of columns)
- Changing schema is slow and costly

-
#### Schemaless databases

- eg: MongoDB
- Flexible semantics of entries in the DB
- Still have an implicit schema (code expects certain properties)
- Schema can change on the fly (requires careful management though)

-
#### Graph Databases

- eg: Neo4J
- Store data as a graph of relationships (eg Facebook's Social graph)


-
-
### Extra Resources

- [Databases on TeachYourselfCS](https://teachyourselfcs.com/#databases)
- Jennifer Widom's Stanford [Database Course](https://www.youtube.com/watch?v=D-k-h0GuFmE&list=PL6hGtHedy2Z4EkgY76QOcueU8lAC4o6c3)