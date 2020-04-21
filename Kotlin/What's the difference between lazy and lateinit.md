# Lazy vs Lateinit

`lateinit` is late initialization. Normally, properties declared as having a non-null type must be initialized in the constructor. However, fairly often this is not convenient. For example, properties can be initialized through dependency injection, or in the setup method of a unit test. In this case, you cannot supply a non-null initializer in the constructor, but you still want to avoid null checks when referencing the property inside the body of a class.
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

`lazy()` is a function that takes a lambda and returns an instance of `Lazy<T>` which can serve as a delegate for implementing a lazy property: the first call to `get()` executes the lambda passed to `lazy()` and remembers the result, subsequent calls to `get()` simply return the remembered result.

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

| lazy | lateinit |
|---|---|
| Can be used with primitive types | Can't be used with primitive types |
| Can only be used for val | Can only be used for var |
| Thread safe by default. Guarantees that the initializer is invoked at most once | Non thread safe. User is responsible for initialize in multi-thread inveronments |
| Can only be initialized from the initializer lambda | Can be initialized from anywhere the object |

## Links
https://blog.mindorks.com/learn-kotlin-lateinit-vs-lazy  
https://www.bignerdranch.com/blog/kotlin-when-to-use-lazy-or-lateinit/  
https://stackoverflow.com/questions/36623177/kotlin-property-initialization-using-by-lazy-vs-lateinit  
