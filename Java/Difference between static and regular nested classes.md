# Inner classes vs Static nested classes

The Java programming language allows you to define a class within another class. Such a class is called a nested class and is illustrated here:

```
class OuterClass {
    ...
    class NestedClass {
        ...
    }
}
```

**Terminology**: Nested classes are divided into two categories: *static* and *non-static*. Nested classes that are declared `static` are called *static nested classes*. Non-static nested classes are called *inner classes*.

Example: 

```
class OuterClass {
    ...
    static class StaticNestedClass {
        ...
    }
    class InnerClass {
        ...
    }
}
```
A nested class is a member of its enclosing class. Non-static nested classes (inner classes) have access to other members of the enclosing class, even if they are declared private. Static nested classes do not have access to other members of the enclosing class. As a member of the `OuterClass`, a nested class can be declared `private`, `public`, `protected`, or *package private*. (Recall that outer classes can only be declared `public` or *package private*.)

## Why Use Nested Classes?
Compelling reasons for using nested classes include the following:
- **It is a way of logically grouping classes that are only used in one place**: If a class is useful to only one other class, then it is logical to embed it in that class and keep the two together. Nesting such "helper classes" makes their package more streamlined.
- **It increases encapsulation**: Consider two top-level classes, A and B, where B needs access to members of A that would otherwise be declared `private`. By hiding class B within class A, A's members can be declared private and B can access them. In addition, B itself can be hidden from the outside world.
- **It can lead to more readable and maintainable code**: Nesting small classes within top-level classes places the code closer to where it is used.

## Static Nested Classes
As with class methods and variables, a static nested class is associated with its outer class. And like static class methods, a static nested class cannot refer directly to instance variables or methods defined in its enclosing class: it can use them only through an object reference.

**Note**: A static nested class interacts with the instance members of its outer class (and other classes) just like any other top-level class. In effect, a static nested class is behaviorally a top-level class that has been nested in another top-level class for packaging convenience.

Static nested classes are accessed using the enclosing class name:
`OuterClass.StaticNestedClass`

For example, to create an object for the static nested class, use this syntax:
`OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();`

## Inner Classes
As with instance methods and variables, an inner class is associated with an instance of its enclosing class and has direct access to that object's methods and fields. Also, because an inner class is associated with an instance, it cannot define any static members itself.

Objects that are instances of an inner class exist *within* an instance of the outer class. Consider the following classes:

```
class OuterClass {
    ...
    class InnerClass {
        ...
    }
}
```

An instance of `InnerClass` can exist only within an instance of `OuterClass` and has direct access to the methods and fields of its enclosing instance.

To instantiate an inner class, you must first instantiate the outer class. Then, create the inner object within the outer object with this syntax:
`OuterClass.InnerClass innerObject = outerObject.new InnerClass();`

| Inner classes  | Static nested classes |
|---|---|
| Without existing outer class object there is no chance of existing inner class object. That is inner class object is always associated with outer class object.  | Without existing outer class object there may be a chance of existing static nested class object. That is static nested class object is not associated with outer class object.  |
| Inside inner class static members can’t be declared.  | Inside static nested class static members can be declared.  |
| As `main()` method can’t be declared, regular inner class can’t be invoked directly from the command prompt.  | As `main()` method can be declared, static nested class can be invoked directly from the command prompt.  |
| Both static and non static members of outer class can be accessed directly.  | Only static member of outer class can be accessed directly.  |

## Links
https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html  
https://www.geeksforgeeks.org/nested-classes-java    
http://tutorials.jenkov.com/java/nested-classes.html       
