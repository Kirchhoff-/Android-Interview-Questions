# `lazy` vs `lateinit`

## `lazy()`
[`lazy()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/lazy.html) is a function that takes a lambda and returns an instance of `Lazy<T>`, which can serve as a delegate for implementing a lazy property. The first call to `get()` executes the lambda passed to `lazy()` and remembers the result. Subsequent calls to `get()` simply return the remembered result.

```
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}

fun main() {
    println(lazyValue)
    println(lazyValue)
}
```

Output:
```
computed!
Hello
Hello
```

By default, the evaluation of lazy properties is **synchronized**: the value is computed only in one thread, but all threads will see the same value.<sup>[1](https://kotlinlang.org/docs/delegated-properties.html#lazy-properties:~:text=lazy()%20is,the%20same%20value.)</sup> 

## `lateinit`
The `lateinit` keyword in Kotlin enables the declaration of non-nullable properties without immediate initialization. This is beneficial when dealing with properties that necessitate a context or extensive initialization that cannot be done during object creation. However, it is crucial to initialize `lateinit` properties before accessing them, as failing to do so will result in an `UninitializedPropertyAccessException`. Let's examine a code sample to better understand this concept:
```
class DemoClass {
    lateinit var userAge: UserAge

    fun init() {
        userAge = UserAge(27)
    }

    fun printAge() {
        if (::age.isInitialized) {
            println("Age is : ${userAge.age}")
        } else {
            println("Age not initialized")
        }
    }
}
```

In the provided code snippet, a property called `age` is declared using the `lateinit` keyword. The `init()` function is responsible for initializing this property. The code then checks if the property has been initialized using the `isInitialized` property reference. If it is initialized, the value of `age` is printed; otherwise, a message indicating that it has not been initialized yet is displayed.<sup>[2](https://rommansabbir.com/kotlin-lateinit-vs-lazy#heading-what-is-lateinit:~:text=their%20Kotlin%20projects.-,What%20is%20lateinit%3F,message%20indicating%20that%20it%20has%20not%20been%20initialized%20yet%20is%20displayed.,-What%20is%20lazy)</sup> 

## [Differences](https://stackoverflow.com/a/36623703/4751612)
| `lazy` | `lateinit` |
|---|---|
| Can be used with primitive types | Can't be used with primitive types |
| Can be used for nullable property | Can't be used for nullable property |
| Can only be used for `val` | Can only be used for `var` |
| Thread safe by default. Guarantees that the initializer is invoked at most once | Non thread safe. User is responsible for initialize in multi-thread inveronments |
| Can only be initialized from the initializer lambda | Can be initialized from anywhere |

# Links 
[Delegated properties﻿](https://kotlinlang.org/docs/delegated-properties.html)

[Kotlin: `lateinit` vs `lazy`](https://rommansabbir.com/kotlin-lateinit-vs-lazy)

[Property initialization using `by lazy` vs. `lateinit`](https://stackoverflow.com/questions/36623177/property-initialization-using-by-lazy-vs-lateinit/36623703)

# Further reading
[Initializing `lazy` and `lateinit` variables in Kotlin](https://blog.logrocket.com/initializing-lazy-lateinit-variables-kotlin/)
