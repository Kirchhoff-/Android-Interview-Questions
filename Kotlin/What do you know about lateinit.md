# `lateinit`
`lateinit` is a keyword used to define a property that will be initialized later. Unlike other properties declared with `var`, `lateinit` properties are not initialized at the time of object creation. This makes it particularly useful when the initial value of the property is not known at the time of declaration.<sup>[1](https://www.dhiwise.com/post/kotlin-lateinit-a-complete-guide-to-late-initialization#:~:text=lateinit%20is%20a,time%20of%20declaration)</sup>

Example of `lateinit` usage:
```
public class MyTest {
    lateinit var subject: TestSubject

    @SetUp fun setup() {
        subject = TestSubject()
    }

    @Test fun test() {
        subject.method()  // dereference directly
    }
}
```

## [Key features of `lateinit` modifier](https://www.dhiwise.com/post/kotlin-lateinit-a-complete-guide-to-late-initialization#:~:text=avoid%20null%20checks.-,Key%20Features%20of%20Lateinit%20Modifier,-Avoid%20Null%20Checks)
- **Avoid Null Checks**: `lateinit` allows you to avoid cumbersome null checks when working with nullable types. The `lateinit` keyword tells the compiler that you will ensure the property is initialized before usage;
- **No Initial Value Required**: When using `lateinit`, you don't need to provide an initial value when the property is declared. This is particularly useful for properties that are not immediately initialized, such as UI components in Android;
- **Easily Set Up Late Initialized Properties**: The `lateinit` modifier simplifies initializing properties in setup methods, unit tests, or other methods where the property can be assigned values dynamically.

## Key restrictions
- Сan only be used with mutable properties (variables declared with the `var` keyword), cause read-only properties (declared with the `val` keyword) represent immutable values that must be initialized at the time of declaration;
- Can be used only for non-primitive types (`Int`, `Double`, `Float` etc not allowed);
- Cannot be used with nullable types, as it is designed to avoid dealing with `null` values;

## [Checking whether a lateinit var is initialized﻿](https://kotlinlang.org/docs/properties.html#checking-whether-a-lateinit-var-is-initialized)
To check whether a `lateinit var` has already been initialized, use `.isInitialized` on the reference to that property:
```
if (foo::bar.isInitialized) {
    println(foo.bar)
}
```

This check is only available for properties that are lexically accessible when declared in the same type, in one of the outer types, or at top level in the same file.

## [Uses of Lateinit in Kotlin](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-lateinit#:~:text=it%27s%20properly%20initialized.-,Uses%20of%20Lateinit%20in%20Kotlin,-Lateinit%20in%20Kotlin)
- **Dependency Injection**. In many dependency injection frameworks, components or services may not be available at the time of object creation. By using `lateinit`, you can declare properties for these dependencies and initialize them when they become available;
- **UI components**. `lateinit` is commonly used for UI components like `TextView`, `Button`, or `EditText`, as they often need to be initialized in the `onCreate` method after the layout is inflated;
- **Testing**. When writing tests, you might want to create an instance of a class for testing but don't have access to all the data necessary for its properties during initialization. `lateinit` allows you to set up the test environment and then initialize the properties as needed;
- **Lazy Initialization**. You can use `lateinit` in conjunction with the `by lazy` delegate to perform lazy initialization of a property. This is useful when the initialization process is costly and should be deferred until the property is first accessed;
- **Data Fetching and Asynchronous Operations**. When working with data that is fetched asynchronously, such as from a network request, you might not have the data available at object creation. `lateinit` can be used to hold the data until it's fetched and then assigned;
- **Custom Initialization Logic**. Some properties may require custom initialization logic that cannot be performed in the constructor. By using `lateinit`, you can define your own initialization methods and call them when the property is ready to be set.

# Links
[Late-initialized properties and variables﻿](https://kotlinlang.org/docs/properties.html#late-initialized-properties-and-variables)

[Kotlin lateinit (Late Initialization): Full Guide With Examples](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-lateinit)

[Kotlin Lateinit: A Complete Guide to Mastering Late Initialization in Kotlin](https://www.dhiwise.com/post/kotlin-lateinit-a-complete-guide-to-late-initialization)

# Next questions
[What is the difference between `lazy` and `lateinit`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20is%20the%20difference%20between%20lazy%20and%20lateinit.md)

# Further reading
[Lateinit and Lazy Property in Kotlin](https://medium.com/@guruprasadhegde4/lateinit-and-lazy-property-in-kotlin-8776c67878a0)
