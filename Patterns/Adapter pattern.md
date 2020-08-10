# Adapter pattern
Adapter pattern is a software design pattern that allows the interface of an existing class to be used as another interface. It is often used to make existing classes work with others without modifying their source code.

The adapter design pattern solves problems like:
- How can a class be reused that does not have an interface that a client requires?
- How can classes that have incompatible interfaces work together?
- How can an alternative interface be provided for a class?

Often an (already existing) class can't be reused only because its interface doesn't conform to the interface clients require.
The adapter design pattern describes how to solve such problems:
- Define a separate `adapter` class that converts the (incompatible) interface of a class (`adaptee`) into another interface (`target`) clients require;
- Work through an `adapter` to work with (reuse) classes that do not have the required interface.

The key idea in this pattern is to work through a separate `adapter` that adapts the interface of an (already existing) class without changing it. Clients don't know whether they work with a `target` class directly or through an `adapter` with a class that does not have the `target` interface.

Let's consider the example:

We have interface `User`:
```
public interface User {
    String name();
    String secondName();
    int id();
}
```

and implemntation of `User` interface - `SimpleUser`:
```
public final class SimpleUser implements User {
    
    private final String name;
    private final String secondName;
    private final int id;
    
    public SimpleUser(String name, String secondName, int id) {
        this.name = name;
        this.secondName = secondName;
        this.id = id;
    }
    
    @Override
    public String name() {
        return name;
    }

    @Override
    public String secondName() {
        return secondName;
    }

    @Override
    public int id() {
        return id;
    }
}
```

Also we have `DbEntity` interface: 
```
public interface DbEntity {
    int id();
    String information();
}
```

And class `Database` which can save `DbEntity` to database: 
```
public final class Database {
    
    void saveEntity(DbEntity entity) {
        //save entity to database
    }
}
```

And we need to save instance of `SimpleUser` to database, but we don't want to or have no chance to make class `SimpleUser` implemented `DbEntity` interface. For resolve such situation we create new class - `SimpleUserAdapter`:
```
public final class SimpleUserAdapter implements DbEntity {

    private final SimpleUser user;

    public SimpleUserAdapter(SimpleUser user){
        this.user = user;
    }

    @Override
    public int id() {
        return user.id();
    }

    @Override
    public String information() {
        return user.name() + " " + user.secondName();
    }
}
```

And we can save `SimpleUser` to database with help of `SimpleUserAdapter`:
```
void saveUserToDatabase() {
    Database database = new Database();
    User user = new SimpleUser("Name", "Second name", 1);
    database.saveEntity(new SimpleUserAdapter(user));
}
``` 

Advantages:
- Helps achieve reusability and flexibility;
- Client class is not complicated by having to use a different interface and can use polymorphism to swap between different implementations of adapters.

Disadvantages:
- All requests are forwarded, so there is a slight increase in the overhead;
- Sometimes many adaptations are required along an adapter chain to reach the type which is required.

## Links
https://en.wikipedia.org/wiki/Adapter_pattern  
https://www.geeksforgeeks.org/adapter-pattern/  
https://www.journaldev.com/1487/adapter-design-pattern-java  
https://dzone.com/articles/adapter-design-pattern-in-java
