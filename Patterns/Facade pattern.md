# Facade pattern
Facade is an object that serves as a front-facing interface masking more complex underlying or structural code. A facade can:
- improve the readability and usability of a software library by masking interaction with more complex components behind a single (and often simplified) API
- provide a context-specific interface to more generic functionality (complete with context-specific input validation)
- serve as a launching point for a broader refactor of monolithic or tightly-coupled systems in favor of more loosely-coupled code

Developers often use the facade design pattern when a system is very complex or difficult to understand because the system has many interdependent classes or because its source code is unavailable. This pattern hides the complexities of the larger system and provides a simpler interface to the client. It typically involves a single wrapper class that contains a set of members required by the client. These members access the system on behalf of the facade client and hide the implementation details.

What problems can the Facade design pattern solve?
- To make a complex subsystem easier to use, a simple interface should be provided for a set of interfaces in the subsystem.
- The dependencies on a subsystem should be minimized.

The facade pattern is typically used when
- a simple interface is required to access a complex system;
- a system is very complex or difficult to understand;
- an entry point is needed to each level of layered software;
- the abstractions and implementations of a subsystem are tightly coupled.

Example:
We have class `DbEntity`:

```
public class DbEntity {
    private String information;
    
    public DbEntity(String information) {
        this.information = information;
    }
}
```

And interface `Database`:
```
public interface Database {
    void saveEntity(DbEntity entity);
}
```

and few implementation of `Database` interface:

`SQLDatabase`:
```
public class SQLDatabase implements Database {
    @Override
    public void saveEntity(DbEntity entity) {
        //Save entity to SQL database
    }
}
```

`NoSQLDatabase`:
```
public class NoSQLDatabase implements Database {
    @Override
    public void saveEntity(DbEntity entity) {
        //Save entity to No SQL Database
    }
}
```

`InMemoryDatabase`:
```
public class InMemoryDatabase implements Database {
    @Override
    public void saveEntity(DbEntity entity) {
        //Save entity to in memory database
    }
}
```

Then we created facade class for database:
```
public class DatabaseFacade {
    
    private SQLDatabase sqlDatabase;
    private NoSQLDatabase noSQLDatabase;
    private InMemoryDatabase inMemoryDatabase;
    
    public DatabaseFacade() {
        sqlDatabase = new SQLDatabase();
        noSQLDatabase = new NoSQLDatabase();
        inMemoryDatabase = new InMemoryDatabase();
    }
    
    void saveToSqlDatabase(DbEntity entity) {
        sqlDatabase.saveEntity(entity);
    }
    
    void saveToNoSqlDatabase(DbEntity entity) {
        noSQLDatabase.saveEntity(entity);
    }
    
    void saveToInMemoryDatabase(DbEntity entity) {
        inMemoryDatabase.saveEntity(entity);
    }
}
```

Usage of `DatabaseFacade`:
```
public class App {
    
    void facadeExample() {
        DbEntity entity = new DbEntity("Information");
        DatabaseFacade facade = new DatabaseFacade();
        
        facade.saveToSqlDatabase(entity);
        facade.saveToNoSqlDatabase(entity);
        facade.saveToInMemoryDatabase(entity);
    }
}
```

Challenges with facade design pattern:
- Subsystems are connected with the facade layer. So, you need to take care of an additional layer of coding;
- When the internal structure of a subsystem changes, you need to incorporate the changes in the facade layer also.

## Links
https://en.wikipedia.org/wiki/Facade_pattern  
https://howtodoinjava.com/design-patterns/structural/facade-design-pattern/  
https://www.geeksforgeeks.org/facade-design-pattern-introduction/  
https://www.baeldung.com/java-facade-pattern
