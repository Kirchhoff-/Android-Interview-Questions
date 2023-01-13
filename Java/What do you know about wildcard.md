# Wildcard
In generic code, the question mark `?`, called the **wildcard**, represents an unknown type. The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type (though it is better programming practice to be more specific). The wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.

## The Basic Generic Collection Assignment Problem
Imagine you have the following class hierarchy:
```
public class A { }
public class B extends A { }
public class C extends A { }
```

The classes `B` and `C` both inherit from `A`.

Then look at these two `List` variables:
```
List<A> listA = new ArrayList<A>();
List<B> listB = new ArrayList<B>();
```

Can you set `listA` to point to `listB` ? or set `listB` to point to `listA`? In other words, are these assignments valid:
```
listA = listB;

listB = listA;
```

The answer is **no** in both cases. Here is why:
In `listA` you can insert objects that are either instances of `A`, or subclasses of `A` (`B` and `C`). If you could do this:
```
List<B> listB = listA;
```

then you could risk that `listA` contains non-`B` objects. When you then try to take objects out of `listB` you could risk to get non-`B` objects out (e.g. an `A` or a `C`). That breaks the contract of the `listB` variable declaration.

Assigning `listB` to `listA` also poses a problem. This assignment, more specifically:
```
listA = listB;
```

if you could make this assignment, it would be possible to insert `A` and `C` instances into the `List<B>` pointed to by `listB`. You could do that via the `listA` reference, which is declared to be of `List<A>`. Thus you could insert non-`B` objects into a list declared to hold `B` (or `B` subclass) instances.

## When are Such Assignments Needed?
The need for making assignments of the type shown earlier in this text arises when creating reusable methods that operate on collections of a specific type.

Imagine you have a method that processes the elements of a `List`, e.g. print out all elements in the `List`. Here is how such a method could look:
```
public void processElements(List<A> elements){
   for(A o : elements){
      System.out.println(o.getValue());
   }
}
```

This method iterates a list of `A` instances, and calls the `getValue()` method (imagine that the class `A` has a method called `getValue()`).

As we have already seen earlier in this text, you can not call this method with a `List<B>` or a `List<C>` typed variable as parameter.

## Generic Wildcards
The generic wildcard operator is a solution to the problem explained above. The generic wildcards target two primary needs:
- Reading from a generic collection;
- Inserting into a generic collection.

There are three ways to define a collection (variable) using generic wildcards. These are:
```
List<?>           listUknown = new ArrayList<A>();
List<? extends A> listUknown = new ArrayList<A>();
List<? super   A> listUknown = new ArrayList<A>();
```

## The Unknown Wildcard
`List<?>` means a list typed to an unknown type. This could be a `List<A>`, a `List<B>`, a `List<String>` etc.

Since the you do not know what type the `List` is typed to, you can only read from the collection, and you can only treat the objects read as being `Object` instances. Here is an example:
```
public void processElements(List<?> elements){
   for(Object o : elements){
      System.out.println(o);
   }
}
```

The `processElements()` method can now be called with any generic `List` as parameter. For instance a `List<A>`, a `List<B>`, `List<C>`, a `List<String>` etc. Here is a valid example:
```
List<A> listA = new ArrayList<A>();

processElements(listA);
```

## Bounded wildcards
A bounded wildcard is one with either an upper or a lower inheritance constraint. The bound of a wildcard can be either a class type, interface type, array type, or type variable. Upper bounds are expressed using the `extends` keyword and lower bounds using the `super` keyword. Wildcards can state either an upper bound *or* a lower bound, but not both.

### Upper bounds
An upper bound on a wildcard must be a subtype of the upper bound of the corresponding type parameter declared in the corresponding generic type. An example of a wildcard that explicitly states an upper bound is:
```
Generic<? extends SubtypeOfUpperBound> referenceConstrainedFromAbove;
```

This reference can hold any parameterization of `Generic` whose type argument is a subtype of `SubtypeOfUpperBound`.  A wildcard that does not explicitly state an upper bound is effectively the same as one that has the constraint `extends Object`, since all reference types in Java are subtypes of `Object`.

### Lower bounds
A wildcard with a lower bound, such as
```
Generic<? super SubtypeOfUpperBound> referenceConstrainedFromBelow;
```

can hold any parameterization of `Generic` whose any type argument is both a subtype of the corresponding type parameter's upper bound and a supertype of `SubtypeOfUpperBound`.

### Bounded wildcards example
In the Java Collections Framework, the class `List<MyClass>` represents an ordered collection of objects of type `MyClass`. Upper bounds are specified using `extends`: A `List<? extends MyClass>` is a list of objects of some subclass of `MyClass`, i.e. any object in the list is guaranteed to be of type `MyClass`, so one can iterate over it using a variable of type `MyClass`:
```
public void doSomething(List<? extends MyClass> list) {
    for (MyClass object : list) { // OK
        // do something
    }
}
```

However, it is not guaranteed that one can add any object of type `MyClass` to that list:
```
public void doSomething(List<? extends MyClass> list) {
    MyClass m = new MyClass();
    list.add(m); // Compile error
}
```

The converse is true for lower bounds, which are specified using `super`: A `List<? super MyClass>` is a list of objects of some superclass of `MyClass`,  i.e. the list is guaranteed to be able to contain any object of type `MyClass`, so one can add any object of type `MyClass`:
```
public void doSomething(List<? super MyClass> list) {
    MyClass m = new MyClass();
    list.add(m); // OK
}
```

However, it is not guaranteed that one can iterate over that list using a variable of type `MyClass`:
```
public void doSomething(List<? super MyClass> list) {
    for (MyClass object : list) { // Compile error
        // do something
    }
}
```

In order to be able to do both add objects of type `MyClass` to the list and iterate over it using a variable of type `MyClass`, a `List<MyClass>` is needed, which is the only type of `List` that is both `List<? extends MyClass>` and `List<? super MyClass>`.

# Links
[Java Generic's Wildcards](https://jenkov.com/tutorials/java-generics/wildcards.html)

[Wildcards](https://docs.oracle.com/javase/tutorial/java/generics/wildcards.html)

[Wildcard (Java)](https://en.wikipedia.org/wiki/Wildcard_(Java))

# Further Reading
[Understanding Contravariance — The Java Wildcard](https://betterprogramming.pub/understanding-contravariance-the-java-wildcard-149853da1559)
