# Nested Class vs Inner Class

**Nested class** is such class which is created inside another class. In Kotlin, nested class is by default **static**, so its data member and member function can be accessed without creating an object of class. Nested class cannot be able to access the data member of outer class.

```
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```

**Inner class** is a class which is created inside another class with keyword **inner**. In other words, we can say that a nested class which is marked as "inner" is called inner class. Inner classes carry a reference to an object of an outer class:

```
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```

Kotlin classes are much similar to Java classes when we think about the capabilites and use cases, but not identical. Nested in Koltin is similar to static nested class in Java and Inner class is similar to non-static nested class in Java.

| Kotlin | Java |
|---|---|
| Nested class | Static Nested class  |
| Inner class  | Non-static nested class |

## Links
https://kotlinlang.org/docs/reference/nested-classes.html  
https://www.javatpoint.com/kotlin-nested-class-and-inner-class  
https://www.geeksforgeeks.org/kotlin-nested-class-and-inner-class/  
