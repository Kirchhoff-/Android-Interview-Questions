# Functional (SAM) interfaces

An interface with only one abstract method is called a **functional interface**, or a **Single Abstract Method (SAM) interface**. The functional interface can have several non-abstract members but only one abstract member.

To declare a functional interface in Kotlin, use the `fun` modifier.<sup>[1](https://kotlinlang.org/docs/fun-interfaces.html#:~:text=An%20interface%20with,the%20fun%20modifier.)</sup>

```
fun interface KRunnable {
   fun invoke()
}
```

## [SAM conversions﻿](https://kotlinlang.org/docs/fun-interfaces.html#sam-conversions)

For functional interfaces, you can use SAM conversions that help make your code more concise and readable by using lambda expressions.

Instead of creating a class that implements a functional interface manually, you can use a lambda expression. With a SAM conversion, Kotlin can convert any lambda expression whose signature matches the signature of the interface's single method into the code, which dynamically instantiates the interface implementation.

For example, consider the following Kotlin functional interface:
```
fun interface IntPredicate {
   fun accept(i: Int): Boolean
}
```

If you don't use a SAM conversion, you will need to write code like this:
```
// Creating an instance of a class
val isEven = object : IntPredicate {
   override fun accept(i: Int): Boolean {
       return i % 2 == 0
   }
}
```

By leveraging Kotlin's SAM conversion, you can write the following equivalent code instead:
```
// Creating an instance using lambda
val isEven = IntPredicate { it % 2 == 0 }
```

A short lambda expression replaces all the unnecessary code:
```
fun interface IntPredicate {
   fun accept(i: Int): Boolean
}

val isEven = IntPredicate { it % 2 == 0 }

fun main() {
   println("Is 7 even? - ${isEven.accept(7)}")
}
```

## `@FunctionalInterface` annotation
`@FunctionalInterface` Annotation:
- This annotation is not mandatory but helps indicate that an interface is intended to be a functional interface;
- It provides compile-time checking to ensure the interface adheres to the single abstract method constraint.<sup>[2](https://www.droidcon.com/2024/05/31/everything-you-want-to-know-about-functional-interfaces-in-kotlin/#:~:text=This%20annotation%20is,abstract%20method%20constraint.)</sup>

```
@FunctionalInterface
interface MyFunctionalInterface {
    fun performAction(name: String)
}
```

## [Functional Interfaces with Default Methods](https://www.droidcon.com/2024/05/31/everything-you-want-to-know-about-functional-interfaces-in-kotlin/#:~:text=Functional%20Interfaces%20with%20Default%20Methods)
Functional interfaces can have default methods, which provide additional functionality without breaking the single abstract method rule:
```
@FunctionalInterface
interface Worker {
    fun work(task: String)

    // Default method
    fun rest() {
        println("Taking a break")
    }
}

fun main() {
    val worker: Worker = Worker { task ->
        println("Working on $task")
    }

    worker.work("Project")
    worker.rest() // Calling the default method
}
```

## [Summary](https://www.droidcon.com/2024/05/31/everything-you-want-to-know-about-functional-interfaces-in-kotlin/#:~:text=functional%20programming%20constructs.-,Summary,-Functional%20interfaces%20in)
Functional interfaces in Kotlin are interfaces with a single abstract method, designed to facilitate functional programming and enhance Java interoperability. Key features include:
- Single Abstract Method (SAM): Ensures the interface has only one abstract method;
- `@FunctionalInterface` Annotation: Optional annotation to enforce SAM compliance;
- SAM Conversion: Allows lambdas to be used where functional interfaces are expected;
- Interoperability with Java: Enables seamless use of Java functional interfaces in Kotlin;
- Default Methods: Provides additional functionality without breaking the single abstract method constraint;
- Higher-Order Functions: Supports passing functional interfaces as parameters for flexible and reusable code.

# Links
[Functional (SAM) interfaces﻿](https://kotlinlang.org/docs/fun-interfaces.html)

[Everything you want to know about Functional interfaces in Kotlin](https://www.droidcon.com/2024/05/31/everything-you-want-to-know-about-functional-interfaces-in-kotlin/)

# Further reading
[SAM Conversions in Kotlin](https://www.baeldung.com/kotlin/sam-conversions)
