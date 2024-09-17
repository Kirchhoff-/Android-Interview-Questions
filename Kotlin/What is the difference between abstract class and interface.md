# `abstract class` vs `interface`

An interface in Kotlin is a blueprint of a class. It describes a set of methods that a class agrees to implement. Unlike a class, an interface cannot hold state (it cannot store properties that are initialized). However, Kotlin interfaces can contain default method implementations. Interfaces are declared using the `interface` keyword<sup>[1](https://app.studyraid.com/en/read/2427/49034/interfaces-and-abstract-classes#:~:text=An%20interface%20in,the%20interface%20keyword.)</sup>, example: 
```
interface Clickable {
    fun click()
    fun showOff() = println("I'm clickable!")
}
```

An abstract class, unlike an interface, can hold state (it can have properties that are initialized). Abstract classes are partially defined classes, methods, and properties that must be implemented by subclasses. Abstract classes are declared using the abstract keyword. Abstract methods and properties in an abstract class are not required to have an implementation in the abstract class itself but must be implemented in the subclasses<sup>[2](https://app.studyraid.com/en/read/2427/49034/interfaces-and-abstract-classes#:~:text=An%20abstract%20class,in%20the%20subclasses.)</sup>, example:
```
abstract class Animated {
    abstract fun animate()
    open fun stopAnimating() {
        println("Stopped animating")
    }
    fun animateTwice() {
        animate()
        animate()
    }
}
```

The main differences are: 

| `abstract class`  | `interface`  |
|---|---|
| Can store state  | Can't store state |
| A class can inherit only from one abstract class | A class can implement multiple interfaces |
| Can provide the implementation of interface  | Can't provide the implementation of abstract class  |
| Can extend another class and implement multiple interfaces  | Can extend another interface only |
| Contains constructors  |  Does not contain constructors |

Which should you use, abstract classes or interfaces?<sup>[3](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html#:~:text=Which%20should%20you,inheritance%20of%20type.)</sup>

Consider using abstract classes if any of these statements apply to your situation:
- You want to share code among several closely related classes;
- You expect that classes that extend your abstract class have many common methods or fields, or require access modifiers other than public (such as protected and private).

Consider using interfaces if any of these statements apply to your situation:
- You expect that unrelated classes would implement your interface. For example, the interfaces `Comparable` and `Cloneable` are implemented by many unrelated classes;
- You want to specify the behavior of a particular data type, but not concerned about who implements its behavior;
- You want to take advantage of multiple inheritance of type.

# Links
[Interfaces and Abstract Classes](https://app.studyraid.com/en/read/2427/49034/interfaces-and-abstract-classes)

[Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)

# Further Reading
[Why use Abstract classes (vs. interfaces)?](https://stackoverflow.com/questions/45616548/why-use-abstract-classes-vs-interfaces)

[Abstract Classes and Interfaces in Kotlin: A Complete Guide](https://sagar0-0.medium.com/abstract-classes-and-interfaces-in-kotlin-a-complete-guide-8e6852cd8305)

[What is the difference between an interface and abstract class?](https://stackoverflow.com/questions/1913098/what-is-the-difference-between-an-interface-and-abstract-class)
