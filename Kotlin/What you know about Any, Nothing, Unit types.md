# Any, Nothing, Unit

`Any` the root of the Kotlin class hierarchy. `Any` is the super type of all non-nullable types. `Any` can't hold the `null` value, for holding `null` value you can use `Any?`. Kotlin compiler treats `kotlin.Any` and `java.lang.Object` as two different types, but at runtime they are represented with the same `java.lang.Object` class.

`Nothing`  has no instances. You can use Nothing to represent "a value that never exists": for example, if a function has the return type of Nothing, it means that it never returns (always throws an exception). `Nothing` implicity extend any object that exists.

```
fun fail(message: String): Nothing {
    throw IllegalStateException(message)
}

val address = employee.address ?: fail("${employee.name} has no address defined")
println(address)

// > java.lang.IllegalStateException: John has no address defined
```

`Unit` In Java if we want that a function does return nothing we use `void`, `Unit` is the equivalent in Kotlin.The main characteristics of `Unit` against Java’s `void` are: 
- `Unit` is a type and therefore can be used as a type argument.
- Only one value of this type exists.
- It is returned implicitly. No need of a `return` statement.

## Links
https://itnext.io/kotlin-basics-types-any-unit-and-nothing-674cc858035  
https://proandroiddev.com/kotlins-nothing-type-946de7d464fb  
https://blog.kotlin-academy.com/kotlins-nothing-its-usefulness-in-generics-5076a6a457f7  
https://stackoverflow.com/questions/38761021/does-any-object  
https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-any/  
https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-nothing.html  
https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-unit/
