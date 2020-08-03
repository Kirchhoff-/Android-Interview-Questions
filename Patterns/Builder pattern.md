# Builder pattern
The builder pattern is a design pattern designed to provide a flexible solution to various object creation problems in object-oriented programming. The intent of the Builder design pattern is to separate the construction of a complex object from its representation.

The Builder design pattern solves problems like:
- How can a class (the same construction process) create different representations of a complex object?
- How can a class that includes creating a complex object be simplified?

Creating and assembling the parts of a complex object directly within a class is inflexible. It commits the class to creating a particular representation of the complex object and makes it impossible to change the representation later independently from (without having to change) the class.

The Builder design pattern describes how to solve such problems:
- Encapsulate creating and assembling the parts of a complex object in a separate Builder object;
- A class delegates object creation to a Builder object instead of creating the objects directly.

A class (the same construction process) can delegate to different Builder objects to create different representations of a complex object.

## Example
For example we have class `Account`
```
public class Account {

    private int id;
    private String name;
    private String secondName;

    public Account(int id, String name, String secondName) {
        this.id = id;
        this.name = name;
        this.secondName = secondName;
    }

    //Getters and setters
}
```

We can use it as follows:
```
Account account = Account(1, "First", "Second");
```

Unfortunately such situation is quite rare in production code. More often we have to create object without knows all information about it or added new field to already complex object thus complicating the constructor of the object.

```
public class Account {

    private int id;
    private String name;
    private String secondName;
    private String description;
    private String info;

    public Account(int id, String name, String secondName) {
       this(id, name, secondName, "", "");
    }

    public Account(int id, String name, String secondName, String description, String info) {
        this.id = id;
        this.name = name;
        this.secondName = secondName;
        this.description = description;
        this.info = info;
    }

    //Getters and setters
}
```
This approach can lead to a large number of object constructors and potential incorrect object state. This is where the Builder pattern comes into play.

```
public class Account {

    private int id;
    private String name;
    private String secondName;
    private String description;
    private String info;

    //For block direct instantiations
    private Account() { }

    //Getters and setters
    public static class Builder {
        private int id; //This info is important so pass it to constructor
        private String name;
        private String secondName;
        private String description;
        private String info;
        
        
        public Builder(int id) {
            this.id = id;
        }
        
        public Builder name(String name) {
            this.name = name;
            
            return this; // return itself for creating fluent interface
        }
        
        public Builder secondName(String secondName) {
            this.secondName = secondName;
            
            return this;
        }
        
        public Builder description(String description) {
            this.description = description;
            
            return this;
        }
        
        public Builder info(String info) {
            this.info = info;
            
            return this;
        }
        
        public Account build() {
            Account account = new Account();
            account.id = this.id;
            account.name = this.name;
            account.secondName = this.secondName;
            account.description = this.description;
            account.info = this.info;
            
            return account;
        }
    }
}
```

Now we can create `Account` like this:
```
Account firstAccount = new Account.Builder(1)
                .name("John")
                .secondName("Doe")
                .description("Description")
                .info("Information")
                .build();
```
This give us the opportunity to create object in more flexible way, than via constructor.

Advantages:
- Allows you to vary a product's internal representation;
- Encapsulates code for construction and representation;
- Provides control over steps of construction process.

Disadvantages:
- Requires creating a separate ConcreteBuilder for each different type of product;
- Requires the builder classes to be mutable;
- Dependency injection may be less supported.

## Links
https://en.wikipedia.org/wiki/Builder_pattern  
https://dzone.com/articles/design-patterns-the-builder-pattern  
https://www.journaldev.com/1425/builder-design-pattern-in-java  
https://medium.com/@sergio.igwt/the-builder-pattern-in-java-2c03fc6ccd16  
