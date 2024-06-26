# Extensions
Kotlin provides the ability to extend a class or an interface with new functionality without having to inherit from the class or use design patterns such as *Decorator*. This is done via special declarations called **extensions**.

For example, you can write new functions for a class or an interface from a third-party library that you can't modify. Such functions can be called in the usual way, as if they were methods of the original class. This mechanism is called an **extension function**. There are also **extension properties** that let you define new properties for existing classes.

## [Extension functions](https://kotlinlang.org/docs/extensions.html#extension-functions)
To declare an extension function, prefix its name with a *receiver type*, which refers to the type being extended. The following adds a `swap` function to `MutableList<Int>`:

```
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
```

The `this` keyword inside an extension function corresponds to the receiver object (the one that is passed before the dot). Now, you can call such a function on any `MutableList<Int>`:

```
val list = mutableListOf(1, 2, 3)
list.swap(0, 2) // 'this' inside 'swap()' will hold the value of 'list'
```

This function makes sense for any `MutableList<T>`, and you can make it generic:

```
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
```

You need to declare the generic type parameter before the function name to make it available in the receiver type expression.

## [Extension properties](https://kotlinlang.org/docs/extensions.html#extension-properties)

Kotlin supports extension properties much like it supports functions:

```
val <T> List<T>.lastIndex: Int
    get() = size - 1
```

**Note**. Since extensions do not actually insert members into classes, there's no efficient way for an extension property to have a backing field. This is why *initializers are not allowed for extension properties*. Their behavior can only be defined by explicitly providing getters/setters.

Example: 
```
val House.number = 1 // error: initializers are not allowed for extension properties
```

## [Nullable receiver](https://kotlinlang.org/docs/extensions.html#nullable-receiver)
Note that extensions can be defined with a nullable receiver type. These extensions can be called on an object variable even if its value is `null`, and they can check for `this == null` inside the body.

This way, you can call `toString()` in Kotlin without checking for `null`, as the check happens inside the extension function:

```
fun Any?.toString(): String {
    if (this == null) return "null"
    // after the null check, 'this' is autocast to a non-null type, so the toString() below
    // resolves to the member function of the Any class
    return toString()
}
```

## [Extensions are resolved statically](https://kotlinlang.org/docs/extensions.html#extensions-are-resolved-statically)

Extensions do not actually modify the classes they extend. By defining an extension, you are not inserting new members into a class, only making new functions callable with the dot-notation on variables of this type.

Extension functions are dispatched *statically*, which means they are not virtual by receiver type. An extension function being called is determined by the type of the expression on which the function is invoked, not by the type of the result from evaluating that expression at runtime. For example:

```
open class Shape
class Rectangle: Shape()

fun Shape.getName() = "Shape"
fun Rectangle.getName() = "Rectangle"

fun printClassName(s: Shape) {
    println(s.getName())
}

printClassName(Rectangle())
```

This example prints `Shape`, because the extension function called depends only on the declared type of the parameter `s`, which is the `Shape` class.

If a class has a member function, and an extension function is defined which has the same receiver type, the same name, and is applicable to given arguments, the *member always wins*. For example:

```
class Example {
    fun printFunctionType() { println("Class method") }
}

fun Example.printFunctionType() { println("Extension function") }

Example().printFunctionType()
```

This code prints `Class method`.

However, it's perfectly OK for extension functions to overload member functions that have the same name but a different signature:

```
class Example {
    fun printFunctionType() { println("Class method") }
}

fun Example.printFunctionType(i: Int) { println("Extension function #$i") }

Example().printFunctionType(1)
```

## [Note on visibility](https://kotlinlang.org/docs/extensions.html#note-on-visibility)
Extensions utilize the same visibility modifiers as regular functions declared in the same scope would. For example:
- An extension declared at the top level of a file has access to the other `private` top-level declarations in the same file;
- If an extension is declared outside its receiver type, it cannot access the receiver's `private` or `protected` members.

# Links
[Extensions](https://kotlinlang.org/docs/extensions.html)

# Next questions
[What do you know about Decorator pattern?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Patterns/Decorator%20pattern.md)

# Further reading
[Writing clean models using extensions](https://okkotlin.com/clean-models/)

[Bad Kotlin Extensions](https://krossovochkin.com/posts/2021_01_25_bad_kotlin_extensions/)

[Extension Functions in Kotlin](https://www.baeldung.com/kotlin/extension-methods)
