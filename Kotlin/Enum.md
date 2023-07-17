

# Enum in Kotlin

An `enum` in Kotlin is a special type that represents a group of related constants. It allows you to define a fixed set of values for a variable. Enum constants are typically used to represent a closed set of options or choices.

To define an `enum` in Kotlin, you use the `enum class` keyword, followed by the name of the enum and the list of constant values inside curly braces. For example:

```kotlin
enum class Color {
    RED, GREEN, BLUE
}
```

In the above example, the `Color` enum has three constants: `RED`, `GREEN`, and `BLUE`.

Enums in Kotlin can also have properties and methods, similar to classes. Each enum constant can have its own values for these properties. For example:

```kotlin
enum class Color(val rgb: Int) {
    RED(0xFF0000),
    GREEN(0x00FF00),
    BLUE(0x0000FF)
    
    fun description(): String {
        return when (this) {
            RED -> "Red color"
            GREEN -> "Green color"
            BLUE -> "Blue color"
        }
    }
}
```

In the above example, the `Color` enum has an additional property `rgb` associated with each constant. It also has a method `description()` that provides a textual description of each color.

To access the properties and methods of an enum constant, you simply use dot notation. For example:

```kotlin
val redColor = Color.RED
println(redColor.rgb)           // Output: 16711680
println(redColor.description()) // Output: Red color
```

Enums in Kotlin can be useful in various scenarios, such as representing days of the week, menu options, status codes, and more. They provide a type-safe and concise way to define a fixed set of values.

## Further Reading: 
For more information about enums in Kotlin, you can refer to the following resources:

- [Kotlin Enum Classes](https://kotlinlang.org/docs/enum-classes.html)
- [Enums in Kotlin](https://www.baeldung.com/kotlin/enum)

Feel free to explore these resources to deepen your understanding of enums in Kotlin.
