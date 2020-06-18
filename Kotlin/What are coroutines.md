# Coroutines
A coroutine is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously. Coroutines were added to Kotlin in version 1.3 and are based on established concepts from other languages.

On Android, coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive.

Coroutines offers many benefits over other asynchronous solutions, including the following:
- **Lightweight**. You can run many coroutines on a single thread due to support for suspension, which doesn't block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations.
- **Fewer memory leaks**. Use structured concurrency to run operations within a scope.
- **Built-in cancellation support**. Cancellation is propagated automatically through the running coroutine hierarchy.
- **Jetpack integration**. Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency.

Example: 
```
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L) // non-blocking delay for 1 second (default time unit is ms)
        println("World!") // print after delay
    }
    println("Hello,") // main thread continues while coroutine is delayed
    Thread.sleep(2000L) // block main thread for 2 seconds to keep JVM alive
}

Output:
Hello,
World!
```

Essentially, coroutines are light-weight threads. They are launched with `launch` *coroutine builder* in a context of some `CoroutineScope`. Here we are launching a new coroutine in the `GlobalScope`, meaning that the lifetime of the new coroutine is limited only by the lifetime of the whole application.

The global idea behind coroutines is that  a function can suspend its execution at some point and resume later on. One of the benefits however of coroutines is that when it comes to the developer, writing non-blocking code is essentially the same as writing blocking code. The programming model in itself doesn't really change.

Take for instance the following code:

```
fun postItem(item: Item) {
    launch {
        val token = preparePost()
        val post = submitPost(token, item)
        processPost(post)
    }
}

suspend fun preparePost(): Token {
    // makes a request and suspends the coroutine
    return suspendCoroutine { /* ... */ } 
}
```

This code will launch a long-running operation without blocking the main thread. The `preparePost` is what's called a `suspendable function`, thus the keyword `suspend` prefixing it. What this means as stated above, is that the function will execute, pause execution and resume at some point in time.

- The function signature remains exactly the same. The only difference is `suspend` being added to it. The return type however is the type we want to be returned.
- The code is still written as if we were writing synchronous code, top-down, without the need of any special syntax, beyond the use of a function called `launch` which essentially kicks-off the coroutine
- The programming model and APIs remain the same. We can continue to use loops, exception handling, etc. and there's no need to learn a complete set of new APIs
- It is platform independent. Whether we targeting JVM, JavaScript or any other platform, the code we write is the same. Under the covers the compiler takes care of adapting it to each platform.

## Structured concurrency
There is still something to be desired for practical usage of coroutines. When we use `GlobalScope.launch`, we create a top-level coroutine. Even though it is light-weight, it still consumes some memory resources while it runs. If we forget to keep a reference to the newly launched coroutine, it still runs. What if the code in the coroutine hangs (for example, we erroneously delay for too long), what if we launched too many coroutines and ran out of memory? Having to manually keep references to all the launched coroutines and join them is error-prone.

There is a better solution. We can use structured concurrency in our code. Instead of launching coroutines in the `GlobalScope`, just like we usually do with threads (threads are always global), we can launch coroutines in the specific scope of the operation we are performing.

## Links
https://kotlinlang.org/docs/reference/coroutines-overview.html  
https://kotlinlang.org/docs/reference/coroutines/basics.html  
https://developer.android.com/kotlin/coroutines  
https://kotlinlang.org/docs/tutorials/coroutines/async-programming.html  
