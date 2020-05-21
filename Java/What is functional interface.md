# Functional interface

An interface with exactly one abstract method is called Functional Interface. `@FunctionalInterface` annotation is added so that we can mark an interface as functional interface.

It is not mandatory to use it, but it’s best practice to use it with functional interfaces to avoid addition of extra methods accidentally. If the interface is annotated with `@FunctionalInterface` annotation and we try to have more than one abstract method, it throws compiler error.

From Java 8 onwards, *lambda expressions* can be used to represent the instance of a functional interface. Any interface with a SAM(Single Abstract Method) is a functional interface, and its implementation may be treated as lambda expressions. A functional interface can have any number of default methods. `Runnable`, `ActionListener`, `Comparable` are some of the examples of functional interfaces. Before Java 8, we had to create anonymous inner class objects or implement these interfaces.

Example of functional interface:
```
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

## Examples of function interfaces

### Function
The Java `Function` interface (`java.util.function.Function`) interface is one of the most central functional interfaces in Java. The `Function` interface represents a function (method) that takes a single parameter and returns a single value. Here is how the Function interface definition looks:
```
public interface Function<T,R> {
    public <R> apply(T parameter);
}
```

The only method you have to implement to implement the `Function` interface is the `apply()` method. Here is a Function implementation example:
```
public class AddThree implements Function<Long, Long> {

    @Override
    public Long apply(Long aLong) {
        return aLong + 3;
    }
}
```

### Predicate
The Java `Predicate` interface, `java.util.function.Predicate`, represents a simple function that takes a single value as parameter, and returns true or false. Here is how the `Predicate` functional interface definition looks:
```
public interface Predicate {
    boolean test(T t);
}
```

You can implement the `Predicate` interface using a class, like this:
```
public class CheckForNull implements Predicate {
    @Override
    public boolean test(Object o) {
        return o != null;
    }
}
```

### Suppliers
The `Supplier` functional interface is yet another `Function` specialization that does not take any arguments. It is typically used for lazy generation of values. For instance, let's define a function that squares a `double` value. It will receive not a value itself, but a `Supplier` of this value:
```
public double squareLazy(Supplier<Double> lazyValue) {
    return Math.pow(lazyValue.get(), 2);
}
```
This allows us to lazily generate the argument for invocation of this function using a `Supplier` implementation. This can be useful if the generation of this argument takes a considerable amount of time

### Consumers

As opposed to the `Supplier`, the `Consumer` accepts a generified argument and returns nothing. It is a function that is representing side effects.

For instance, let’s greet everybody in a list of names by printing the greeting in the console. The lambda passed to the `List.forEach` method implements the `Consumer` functional interface:
```
List<String> names = Arrays.asList("John", "Freddy", "Samuel");
names.forEach(name -> System.out.println("Hello, " + name));
```

## Links
https://www.baeldung.com/java-8-functional-interfaces  
http://tutorials.jenkov.com/java-functional-programming/functional-interfaces.html  
https://www.geeksforgeeks.org/functional-interfaces-java/  
https://www.journaldev.com/2763/java-8-functional-interfaces  
