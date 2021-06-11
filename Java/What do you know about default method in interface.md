# Default method in interface
**Java 8 allows to define default method in interface**. Default methods allow us to add new methods in interface that are automatically available to it’s implementation classes by default. This is called method extension in Interfaces.

Until 1.7 version inside `interface` we can declare only `public abstract` methods and `public static final` variables, concrete methods are not allowed. But from Java 1.8 version on wards in addition to these, we can declare default concrete methods also inside interface, which are also known as defender methods. Default methods are allowed only in interfaces. All method declarations in an `interface`, including default methods, are implicitly `public`, so you can omit the `public` modifier. 

We can declare default method with the keyword `default` as follows:
```
public interface MyInterface {
    //default method
    default void wish(String user) {
        System.out.println("Hello "+user+"!");
    }
}
```

## Use of default method in java interfaces
Any class that implements an `interface` must provide an implementation for each method defined by the `interface` or inherit the implementation from a super class, but default methods enable us to add new functionalities to interfaces without breaking the classes that implements that `interface`. Default methods also known as **defender methods** or **virtual extension methods**.

The main advantage of default methods is without effecting implementation classes we can add new functionality to the `interface` (backward compatibility).

Interface default methods are by default available to all implementation classes. **An implementation class can use these default methods directly or can override**. When do override default methods in implementation classes you omit the default keyword.

```
interface PrinterInterface {
    default void display(String user) {
        System.out.println("Hello " + user);
    }
}
```

Implementation of interface:
```
class PrinterImplementation implements PrinterInterface {}
```

Example of usage:
```
public class DefaultMethodExample {
    
    public static void main(String[] args) {
        PrinterImplementation printerImplementation = new PrinterImplementation();
        printerImplementation.display("John Doe");
    }
}
```

Output:
```
Hello John Doe
```

## Multiple Defaults
With default functions in interfaces, there is a possibility that a class is implementing two interfaces with same default methods. The following code explains how this ambiguity can be resolved.

```
public interface Vehicle {

   default void print() {
      System.out.println("I am a vehicle!");
   }
}

public interface FourWheeler {

   default void print() {
      System.out.println("I am a four wheeler!");
   }
}
```

First solution is to create an own method that overrides the default implementation.
```
public class Car implements vehicle, fourWheeler {

   public void print() {
      System.out.println("I am a four wheeler car vehicle!");
   }
}
```

Second solution is to call the default method of the specified interface using super.
```
public class car implements Vehicle, FourWheeler {

   public void print() {
      Vehicle.super.print();
   }
}
```

## [Static Interface Methods](https://www.baeldung.com/java-static-default-methods#static-interface-methods)
Aside from being able to declare *default* methods in interfaces, **Java 8 allows us to define and implement *static* methods in interfaces**.

Since `static` methods don't belong to a particular object, they are not part of the API of the classes implementing the `interface`, and they have to be called by using the interface name preceding the method name.

Defining a `static` method within an `interface` is identical to defining one in a class. Moreover, a `static` method can be invoked within other `static` and `default` methods.

```
public interface Vehicle {

   default void print() {
      System.out.println("I am a vehicle!");
   }
	
   static void blowHorn() {
      System.out.println("Blowing horn!!!");
   }
}
```

The idea behind `static` interface methods is to provide a simple mechanism that allows us to increase the degree of cohesion of a design by putting together related methods in one single place without having to create an object.

Pretty much the same can be done with abstract classes. The main difference lies in the fact that abstract classes can have **constructors, state, and behavior**.

Furthermore, `static` methods in interfaces make possible to group related utility methods, without having to create artificial utility classes that are simply placeholders for static methods.

## Conclusion
- Interfaces can have `default` methods with implementation in Java 8 on later;
- Interfaces can have `static` methods as well, similar to `static` methods in classes;
- Default methods were introduced to provide backward compatibility for old interfaces so that they can have new methods without affecting existing code.

# Links
[Java – Interface default methods](https://javabydeveloper.com/java-default-method-in-interface-or-method-extension/)

[Java 8 - Default Methods](https://www.tutorialspoint.com/java8/java8_default_methods.htm)

[Default Methods In Java 8](https://www.geeksforgeeks.org/default-methods-java/)

[Static and Default Methods in Interfaces in Java](https://www.baeldung.com/java-static-default-methods#static-interface-methods)

# Futer reading
[Default Methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)
