# Immutable object
An immutable object is an object whose internal state remains constant after it has been entirely created. This means that the public API of an immutable object guarantees us that it will behave in the same way during its whole lifetime. Examples of immutable objects from the JDK include String and Integer.

Example: 
```
public final class ImmutableClass {
    
    private final String property1;
    private final String property2;
    
    public ImmutableClass(String property1, String property2) {
        this.property1 = property1;
        this.property2 = property2;
    }
    
    public String getProperty1() {
        return property1;
    }
    
    public String getProperty2() {
        return property2;
    }
}
```

## A strategy for defining immutable objects
- Don't provide "setter" methods — methods that modify fields or objects referred to by fields.
- Make all fields `final` and `private`.
- Don't allow subclasses to `override` methods. The simplest way to do this is to declare the class as `final`. A more sophisticated approach is to make the constructor private and construct instances in factory methods.
- If the instance fields include references to mutable objects, don't allow those objects to be changed:
  - Don't provide methods that modify the mutable objects.
  - Don't share references to the mutable objects. Never store references to external, mutable objects passed to the constructor; if necessary, create copies, and store references to the copies. Similarly, create copies of your internal mutable objects when necessary to avoid returning the originals in your methods.
  
## Benefits of immutable objects
- Thread safety
- Atomicity of failure
- Absence of hidden side-effects
- Protection against null reference errors
- Ease of caching
- Prevention of identity mutation
- Avoidance of temporal coupling between methods
- Support for referential transparency
- Protection from instantiating logically-invalid objects
- Protection from inadvertent corruption of existing objects

## Links
https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html  
https://www.baeldung.com/java-immutable-object  
http://www.javapractices.com/topic/TopicAction.do?Id=29  
https://www.leadingagile.com/2018/03/immutability-in-java/
