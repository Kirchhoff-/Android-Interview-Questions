# Singleton pattern
Singleton pattern is a software design pattern that restricts the instantiation of a class to one "single" instance. This is useful when exactly one object is needed to coordinate actions across the system. The term comes from the mathematical concept of a singleton.

The singleton design pattern solves problems like:
- How can it be ensured that a class has only one instance?
- How can the sole instance of a class be accessed easily?
- How can a class control its instantiation?
- How can the number of instances of a class be restricted?

To create the singleton class, we need to have static member of class, private constructor and static factory method.
- **Static member**: It gets memory only once because of static, itcontains the instance of the Singleton class.
- **Private constructor**: It will prevent to instantiate the Singleton class from outside the class.
- **Static factory metho**d: This provides the global point of access to the Singleton object and returns the instance to the caller.

```
public final class ClassSingleton {
 
    private static ClassSingleton INSTANCE;
    private String info = "Initial info class";
    
    private ClassSingleton() {        
    }
    
    public static ClassSingleton getInstance() {
        if(INSTANCE == null) {
            INSTANCE = new ClassSingleton();
        }
        
        return INSTANCE;
    }
 
    // getters and setters
}
```

Common uses:
- The abstract factory, factory method, builder, and prototype patterns can use singletons in their implementation.
- Facade objects are often singletons because only one facade object is required.
- State objects are often singletons.

## Links
https://en.wikipedia.org/wiki/Singleton_pattern  
https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples  
https://www.geeksforgeeks.org/singleton-class-java/  
https://www.javatpoint.com/singleton-design-pattern-in-java  
https://www.baeldung.com/java-singleton  
https://sourcemaking.com/design_patterns/singleton  
