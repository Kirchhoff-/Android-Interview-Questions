# Data objects
When printing a plain object declaration in Kotlin, the string representation contains both its name and the hash of the `object`:
```
object MyObject

fun main() {
    println(MyObject) 
    // MyObject@hashcode
}
```

Output:
```
MyObject@3941a79c
```

However, by marking an object declaration with the `data` modifier, you can instruct the compiler to return the actual name of the object when calling `toString()`, the same way it works for data classes:
```
data object MyDataObject {
    val number: Int = 3
}

fun main() {
    println(MyDataObject) 
    // MyDataObject
}
```

Output:
```
MyDataObject
```

Additionally, the compiler generates several functions for your `data object`:
- `toString()` returns the name of the data object;
- `equals()`/`hashCode()` enables equality checks and hash-based collections.

**Note**: You can't provide a custom `equals` or `hashCode` implementation for a `data object`.

The `equals()` function for a `data object` ensures that all objects that have the type of your `data object` are considered equal. In most cases, you will only have a single instance of your `data object` at runtime, since a `data object` declares a singleton. However, in the edge case where another object of the same type is generated at runtime (for example, by using platform reflection with `java.lang.reflect` or a JVM serialization library that uses this API under the hood), this ensures that the objects are treated as being equal.

The generated `hashCode()` function has a behavior that is consistent with the `equals()` function, so that all runtime instances of a `data object` have the same hash code.

## [Differences between data objects and data classes﻿](https://kotlinlang.org/docs/object-declarations.html#differences-between-data-objects-and-data-classes)
While `data object` and `data class` declarations are often used together and have some similarities, there are some functions that are not generated for a `data object`:
- No `copy()` function. Because a `data object` declaration is intended to be used as singletons, no `copy()` function is generated. Singletons restrict the instantiation of a class to a single instance, which would be violated by allowing copies of the instance to be created;
- No `componentN()` function. Unlike a `data class`, a `data object` does not have any data properties. Since attempting to destructure such an object without data properties wouldn't make sense, no `componentN()` functions are generated.

## [Use data objects with sealed hierarchies﻿](https://kotlinlang.org/docs/object-declarations.html#use-data-objects-with-sealed-hierarchies)
Data object declarations are particularly useful for sealed hierarchies like sealed classes or sealed interfaces. They allow you to maintain symmetry with any data classes you may have defined alongside the object.

In this example, declaring `EndOfFile` as a `data object` instead of a plain `object` means that it will get the `toString()` function without the need to override it manually:
```
sealed interface ReadResult
data class Number(val number: Int) : ReadResult
data class Text(val text: String) : ReadResult
data object EndOfFile : ReadResult

fun main() {
    println(Number(7)) 
    // Number(number=7)
    println(EndOfFile) 
    // EndOfFile
}
```

# Links
[Data objects](https://kotlinlang.org/docs/object-declarations.html#data-objects)

# Further reading
[Data Objects in Kotlin](https://medium.com/@domen.lanisnik/data-objects-in-kotlin-1a549bfad657)

# Next questions
[What's data classes?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What's%20data%20classes.md)

[What do you know about `companion object`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20companion%20object.md)

[What do you know about `object` keyword?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20object%20keyword.md)

[What do you know about sealed classes and interfaces?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20sealed%20classes%20and%20interfaces.md)
