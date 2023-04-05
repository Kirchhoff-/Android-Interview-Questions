# Interface
There are a number of situations in software engineering when it is important for disparate groups of programmers to agree to a "contract" that spells out how their software interacts. Each group should be able to write their code without any knowledge of how the other group's code is written. Generally speaking, **interfaces** are such contracts.<sup>[1](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html#:~:text=There%20are%20a,are%20such%20contracts.)</sup>

Interfaces are often used to define the contracts between different modules or components of a system, which can then be implemented by classes to form a working system. By using interfaces, we can also benefit from polymorphism, as a class can implement multiple interfaces, each providing its own set of methods and behavior.

Interfaces allow for easier integration of new features as they create a contract that each class should comply with, resulting in fewer errors and increased readability. They also help reduce coupling between classes by providing a standard way for different parts of code to communicate with each other.

Additionally, the use of interfaces in Java provides a reliable mechanism of communication between different parts of an application and allows developers to rely on the same contract even when other parts of the application are changed and updated. <sup>[2](https://www.developer.com/java/java-interfaces/#:~:text=Interfaces%20are%20often,changed%20and%20updated.)</sup>

In the Java programming language, an `interface` is a reference type, similar to a class, that can contain *only* constants method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods. Interfaces cannot be instantiated—they can only be *implemented* by classes or *extended* by other interfaces. <sup>[3](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html#:~:text=In%20the%20Java,in%20this%20lesson.)</sup>

## Defining an Interface
An interface declaration consists of modifiers, the keyword `interface`, the interface name, a comma-separated list of parent interfaces (if any), and the interface body. For example:
```
public interface GroupedInterface extends Interface1, Interface2, Interface3 {

    // constant declarations
    
    // base of natural logarithms
    double E = 2.718282;
 
    // method signatures
    void doSomething (int i, double x);
    int doSomethingElse(String s);
}
```

The `public` access specifier indicates that the interface can be used by any class in any package. If you do not specify that the interface is public, then your interface is accessible only to classes defined in the same package as the interface.

An interface can extend other interfaces, just as a class subclass or extend another class. However, whereas a class can extend only one other class, an interface can extend any number of interfaces. The interface declaration includes a comma-separated list of all the interfaces that it extends. <sup>[4](https://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html#:~:text=An%20interface%20declaration,that%20it%20extends.)</sup>

## [Implementing an Interface](https://docs.oracle.com/javase/tutorial/java/IandI/usinginterface.html)
To declare a class that `implements` an interface, you include an implements clause in the class declaration. Your class can implement more than one interface, so the `implements` keyword is followed by a comma-separated list of the interfaces implemented by the class. By convention, the `implements` clause follows the `extends` clause, if there is one.

### A Sample Interface, `Relatable`
Consider an interface that defines how to compare the size of objects.
```
public interface Relatable {
        
    // this (object calling isLargerThan)
    // and other must be instances of 
    // the same class returns 1, 0, -1 
    // if this is greater than, 
    // equal to, or less than other
    public int isLargerThan(Relatable other);
}
```

If you want to be able to compare the size of similar objects, no matter what they are, the class that instantiates them should implement `Relatable`. Any class can implement `Relatable` if there is some way to compare the relative "size" of objects instantiated from the class.

Here is the Rectangle class that implement `Relatable` interface:
```
public class RectanglePlus implements Relatable {
    public int width = 0;
    public int height = 0;
    public Point origin;

    // four constructors
    public RectanglePlus() {
        origin = new Point(0, 0);
    }
    public RectanglePlus(Point p) {
        origin = p;
    }
    public RectanglePlus(int w, int h) {
        origin = new Point(0, 0);
        width = w;
        height = h;
    }
    public RectanglePlus(Point p, int w, int h) {
        origin = p;
        width = w;
        height = h;
    }
    
    // a method required to implement
    // the Relatable interface
    public int isLargerThan(Relatable other) {
        RectanglePlus otherRect = (RectanglePlus)other;
        if (this.getArea() < otherRect.getArea())
            return -1;
        else if (this.getArea() > otherRect.getArea())
            return 1;
        else
            return 0;               
    }
}
```

Because `RectanglePlus` implements `Relatable`, the size of any two `RectanglePlus` objects can be compared.

## [Using an Interface as a Type](https://docs.oracle.com/javase/tutorial/java/IandI/interfaceAsType.html)
When you define a new interface, you are defining a new reference data type. You can use interface names anywhere you can use any other data type name. If you define a reference variable whose type is an interface, any object you assign to it *must* be an instance of a class that implements the interface.

As an example, here is a method for finding the largest object in a pair of objects, for *any* objects that are instantiated from a class that implements `Relatable`:
```
public Object findLargest(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ((obj1).isLargerThan(obj2) > 0)
      return object1;
   else 
      return object2;
}
```

By casting `object1` to a `Relatable` type, it can invoke the `isLargerThan` method.

## Marker Interfaces
Sometimes it is useful to define an interface that is entirely empty. A class can implement this interface simply by naming it in its `implements` clause without having to implement any methods. In this case, any instances of the class become valid instances of the interface. Java code can check whether an object is an instance of the `interface` using the `instanceof` operator, so this technique is a useful way to provide additional information about an object. The `Cloneable` interface in `java.lang` is an example of this type of marker interface. It defines no methods, but identifies the class as one that allows its internal state to be cloned by the `clone()` method of the `Object` class. As of Java 1.1, `java.io.Serializable` is another such marker interface. Given an arbitrary object, you can determine whether it has a working `clone()` method with code like this<sup>[5](https://docstore.mik.ua/orelly/java-ent/jnut/ch03_07.htm#:~:text=Sometimes%20it%20is,else%20copy%20%3D%20null%3B)</sup>: 
```
Object o; // Initialized elsewhere 
Object copy; 
if (o instanceof Cloneable) copy = o.clone(); 
else copy = null;
``` 

# Links
[Lesson: Interfaces and Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/index.html)

[What are Interfaces in Java](https://www.developer.com/java/java-interfaces/)

[Interfaces](https://docstore.mik.ua/orelly/java-ent/jnut/ch03_07.htm)

# Next questions
[What is the difference between Abstract class and Interface?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/Difference%20between%20Abstract%20class%20and%20Interfaces.md)

[What do you know about default method in interface?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20default%20method%20in%20interface.md)

[What is functional interface?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20is%20functional%20interface.md)

# Further reading
[Java Interfaces](https://jenkov.com/tutorials/java/interfaces.html)

[Interface (Java)](https://en.wikipedia.org/wiki/Interface_(Java))

[Java Interfaces](https://www.baeldung.com/java-interfaces)

[Interface in Java](https://www.scaler.com/topics/java/interface-in-java/)
