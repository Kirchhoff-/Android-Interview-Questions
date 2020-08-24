# State pattern
The state pattern is a behavioral software design pattern that allows an object to alter its behavior when its internal state changes. 

The state pattern is used in computer programming to encapsulate varying behavior for the same object, based on its internal state. This can be a cleaner way for an object to change its behavior at runtime without resorting to conditional statements and thus improve maintainability.

The state pattern is set to solve two main problems:
- An object should change its behavior when its internal state changes;
- State-specific behavior should be defined independently. That is, adding new states should not affect the behavior of existing states.

Implementing state-specific behavior directly within a class is inflexible because it commits the class to a particular behavior and makes it impossible to add a new state or change the behavior of an existing state later independently from (without changing) the class. In this, the pattern describes two solutions:
- Define separate (state) objects that encapsulate state-specific behavior for each state. That is, define an interface (state) for performing state-specific behavior, and define classes that implement the interface for each state;
- A class delegates state-specific behavior to its current state object instead of implementing state-specific behavior directly.

This makes a class independent of how state-specific behavior is implemented. New states can be added by defining new state classes. A class can change its behavior at run-time by changing its current state object.

State pattern drawback is the payoff when implementing transition between the states. That makes the state hardcoded, which is a bad practice in general. But, depending on our needs and requirements, that might or might not be an issue.

## Example
Suppose we have an interface `PackageState`:
```
public interface PackageState {
    PackageState updateState();
    void printStatus();
}
```

and a few implementation for `PackageState` interface: 

```
public final class DeliveredPackageState implements PackageState {

    private final String packageId;

    public DeliveredPackageState(String packageId) {
        this.packageId = packageId;
    }

    @Override
    public PackageState updateState() {
        throw new IllegalStateException("Package with id = " + packageId + " already delivered");
    }

    @Override
    public void printStatus() {
        System.out.println("Package with id = " + packageId + " delivered");
    }
}
```

```
public final class ShippedPackageState implements PackageState {

    private final String packageId;

    public ShippedPackageState(String packageId) {
        this.packageId = packageId;
    }

    @Override
    public PackageState updateState() {
        return new DeliveredPackageState(packageId);
    }

    @Override
    public void printStatus() {
        System.out.println("Package with id = " + packageId + " shipped");
    }
}
```

```
public final class OrderedPackageState implements PackageState {

    private final String packageId;

    public OrderedPackageState(String packageId) {
        this.packageId = packageId;
    }

    @Override
    public PackageState updateState() {
        return new ShippedPackageState(packageId);
    }

    @Override
    public void printStatus() {
        System.out.println("Package with id = " + packageId + " ordered");
    }
}
```

```
public final class InitPackageState implements PackageState {

    private final String packageId;

    public InitPackageState(String packageId) {
        this.packageId = packageId;
    }

    @Override
    public PackageState updateState() {
        return new OrderedPackageState(packageId);
    }

    @Override
    public void printStatus() {
        System.out.println("Package with id = " + packageId + " init");
    }
}
```

Also we have a `PackageManager` class which is responsible for moving from one state to another:
```
public final class PackageManager {

    private PackageState currentPackageState;
    
    public PackageManager(String packageId) {
        currentPackageState = new InitPackageState(packageId);
    }
    
    public void updateState() {
        currentPackageState = currentPackageState.updateState();
    }
    
    public void printStatus() {
        currentPackageState.printStatus();
    }
}
```

Example of usage `PackageManager`:
```
public class StateExample {

    public void example() {
        PackageManager manager = new PackageManager("12345");

        manager.printStatus();

        //After fulfilling some condition
        manager.updateState();
        manager.printStatus();
        
        manager.updateState();
        manager.printStatus();
        
        manager.updateState();
        manager.printStatus();
    }
}
```

Output:
```
Package with id = 12345 init
Package with id = 12345 ordered
Package with id = 12345 shipped
Package with id = 12345 delivered
```

## Links
https://en.wikipedia.org/wiki/State_pattern  
https://www.baeldung.com/java-state-design-pattern  
https://sourcemaking.com/design_patterns/state  
https://howtodoinjava.com/design-patterns/behavioral/state-design-pattern/
