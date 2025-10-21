# `сompanion object`

If you need to write a function that can be called without having a class instance but that needs access to the internals of a class (such as a factory method), you can declare a `companion object` inside your class and access its members using only the class name as a qualifier<sup>[1](https://kotlinlang.org/docs/classes.html#companion-objects:~:text=If%20you%20need,as%20a%20qualifier.)</sup>:

```
class MyClass {
    companion object {
        fun createInstance(): MyClass = MyClass()
    }
}
```

Now we can call the function `createInstance()` using only the class name:
```
val instance = MyClass.createInstance()
```

## [Named Companion Object](https://www.baeldung.com/kotlin/companion-object#named-companion-object)
By default, a companion object’s name is `Companion`. However, it is possible to rename it. Let’s implement a Factory Method design pattern in its simplest form. The Factory Method design pattern handles object creation. We’ll implement this design pattern using a `companion object`. Let’s name its companion object `Factory`:
```
class MyClass {
    companion object Factory {
        fun createInstance(): MyClass = MyClass()
    }
}
```

Now, if we prefer to make use of the `companion object` by the dedicated name, simply place it after the class name:
```
val instance = MyClass.Factory.createInstance()
```

## [Why Use Companion Objects?](https://medium.com/@riztech.dev/understanding-companion-objects-in-kotlin-a93f1a5880a7#:~:text=Why%20Use%20Companion%20Objects%3F)
Companion objects offer several benefits:

### [Namespace for Factory Methods](https://medium.com/@riztech.dev/understanding-companion-objects-in-kotlin-a93f1a5880a7#:~:text=Namespace%20for%20Factory%20Methods)
Companion objects provide a convenient location to define factory methods. Factory methods are static-like methods that create instances of a class. This approach is cleaner and more maintainable compared to using multiple constructors, especially when complex initialization logic is involved:
```
class User private constructor(val name: String) {
    companion object {
        fun newUser(name: String): User {
            return User(name)
        }
    }
}

val user = User.newUser("John Doe")
```

### [Constants and Utility Functions](https://medium.com/@riztech.dev/understanding-companion-objects-in-kotlin-a93f1a5880a7#:~:text=2.-,Constants%20and%20Utility%20Functions,-%3A%20Companion%20objects%20are)
Companion objects are ideal for holding constants and utility functions that are associated with the class but do not require an instance of the class. This keeps your code organized and makes it clear which constants and utilities are related to which class:
```
class Constants {
    companion object {
        const val MAX_AGE = 100
        const val MIN_AGE = 1
    }
}

println(Constants.MAX_AGE) // Output: 100
println(Constants.MIN_AGE) // Output: 1
```

### [Encapsulation](https://medium.com/@riztech.dev/understanding-companion-objects-in-kotlin-a93f1a5880a7#:~:text=3.-,Encapsulation,-%3A%20By%20using)
By using companion objects, you can encapsulate static-like members within the class, reducing global namespace pollution. This helps in keeping your codebase clean and modular, as related functions and constants are grouped together within their respective classes:
```
class Calculator {
    companion object {
        fun add(a: Int, b: Int): Int = a + b
        fun subtract(a: Int, b: Int): Int = a - b
    }
}

val result = Calculator.add(5, 3)
println(result) // Output: 8
```

### [Interoperability with Java](https://medium.com/@riztech.dev/understanding-companion-objects-in-kotlin-a93f1a5880a7#:~:text=4.-,Interoperability%20with%20Java,-%3A%20Kotlin%E2%80%99s%20companion)
Kotlin’s companion objects facilitate interoperability with Java by providing a way to define static members in Kotlin classes. This ensures that Kotlin classes can be used seamlessly in Java code, which is particularly useful in mixed-language projects:
```
class MyClass {
    companion object {
        @JvmStatic
        fun greet() {
            println("Hello from Kotlin!")
        }
    }
}

// In Java
MyClass.greet(); // Calls the companion object's greet method
```

## [Interfaces and Inheritance](https://kotlinlang.org/docs/object-declarations.html#semantic-difference-between-object-expressions-and-declarations:~:text=Note%20that%20even%20though%20the%20members%20of%20companion%20objects%20look%20like%20static%20members%20in%20other%20languages%2C%20at%20runtime%20those%20are%20still%20instance%20members%20of%20real%20objects%2C%20and%20can%2C%20for%20example%2C%20implement%20interfaces%3A)
Even though the members of companion objects look like static members in other languages, at runtime those are still instance members of real objects and also can implement interfaces, inherit from other classes:
```
interface Factory<T> {
    fun create(): T
}

class MyClass {
    companion object : Factory<MyClass> {
        override fun create(): MyClass = MyClass()
    }
}

val f: Factory<MyClass> = MyClass
```

## [Interfaces and Companion Object](https://www.baeldung.com/kotlin/companion-object#:~:text=6.%20Interfaces%20and%20Companion%20Object)
A `companion object` can be used in interfaces as well. We can define properties and concrete functions within a `companion object` enclosed in an `interface`. One potential usage is storing constants and helper functions related to the interface:
```
interface MyInterface {
    companion object {
        const val PROPERTY = "value"
    }
}
```

## Limitations of `companion object`
- Only one companion object is available for each class;
- Cannot create a `companion object` inside another `object`;
- Companion objects can not be nested;
- Companion objects and their members can only be accessed via the containing class name.

# Links
[Classes﻿](https://kotlinlang.org/docs/classes.html)

[Kotlin Companion Object](https://www.baeldung.com/kotlin/companion-object)

[Understanding Companion Objects in Kotlin](https://medium.com/@riztech.dev/understanding-companion-objects-in-kotlin-a93f1a5880a7)

[Kotlin Companion Objects (Explained With Examples)](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-companion-objects)

# Further reading
[A few facts about Companion objects](https://blog.kotlin-academy.com/a-few-facts-about-companion-objects-37e18429b725)

[Companion Objects](https://www.kotlinprimer.com/classes-what-kotlin-brings-to-the-table/objects/companion-objects/)

# Next questions
[What do you know about `object` keyword?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/Object%20keyword.md)
