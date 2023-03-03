# `launch` vs `async`

[`launch`](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html)
```
fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext, 
    start: CoroutineStart = CoroutineStart.DEFAULT, 
    block: suspend CoroutineScope.() -> Unit
): Job
```

Launches a new coroutine without blocking the current thread and returns a reference to the coroutine as a `Job`. The coroutine is cancelled when the resulting job is cancelled. [1](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html#:~:text=Launches%20a%20new%20coroutine%20without%20blocking%20the%20current%20thread%20and%20returns%20a%20reference%20to%20the%20coroutine%20as%20a%20Job.%20The%20coroutine%20is%20cancelled%20when%20the%20resulting%20job%20is%20cancelled.)

[`async`](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html)
```
fun <T> CoroutineScope.async(
    context: CoroutineContext = EmptyCoroutineContext, 
    start: CoroutineStart = CoroutineStart.DEFAULT, 
    block: suspend CoroutineScope.() -> T
): Deferred<T>
```

Creates a coroutine and returns its future result as an implementation of `Deferred`. The running coroutine is cancelled when the resulting deferred is cancelled. The resulting coroutine has a key difference compared with similar primitives in other languages and frameworks: it cancels the parent job (or outer scope) on failure to enforce *structured concurrency* paradigm. To change that behaviour, supervising parent (`SupervisorJob` or `supervisorScope`) can be used. [2](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html#:~:text=Creates%20a%20coroutine,can%20be%20used.)

The difference is that the `launch{}` returns a `Job` and does not carry any resulting value whereas the `async{}` returns an instance of `Deferred<T>`, which has an `await()` function that returns the result of the coroutine. [3](https://amitshekhar.me/blog/launch-vs-async-in-kotlin-coroutines#:~:text=The%20difference%20is%20that%20the%20launch%7B%7D%20returns%20a%20Job%20and%20does%20not%20carry%20any%20resulting%20value%20whereas%20the%20async%7B%7D%20returns%20an%20instance%20of%20Deferred%3CT%3E%2C%20which%20has%20an%20await()%20function%20that%20returns%20the%20result%20of%20the%20coroutine)

```
suspend fun example() {
        viewModelScope.launch {
            //Perform some task (Not possible to return result)
        }

        val asyncDeferred = viewModelScope.async {
            //Perform some task (Result can be returned)
            return@async 3
        }
        val result = asyncDeferred.await() //equals 3
    }
```

Typically, you should `launch` a new coroutine from a regular function, as a regular function cannot call `await`. Use `async` only when inside another coroutine or when inside a suspend function and performing parallel decomposition. [4](https://developer.android.com/kotlin/coroutines/coroutines-adv#:~:text=Typically%2C%20you%20should%20launch%20a%20new%20coroutine%20from%20a%20regular%20function%2C%20as%20a%20regular%20function%20cannot%20call%20await.%20Use%20async%20only%20when%20inside%20another%20coroutine%20or%20when%20inside%20a%20suspend%20function%20and%20performing%20parallel%20decomposition.)

**Warning**. `launch` and async handle exceptions differently. Since `async` expects an eventual call to `await`, it holds exceptions and rethrows them as part of the `await` call. This means if you use `async` to start a new coroutine from a regular function, you might silently drop an exception. These dropped exceptions won't appear in your crash metrics or be noted in logcat. [5](https://developer.android.com/kotlin/coroutines/coroutines-adv#:~:text=Warning%3A%20launch,in%20Coroutines.)

# Links
[Launch vs Async in Kotlin Coroutines](https://amitshekhar.me/blog/launch-vs-async-in-kotlin-coroutines)

[Improve app performance with Kotlin coroutines](https://developer.android.com/kotlin/coroutines/coroutines-adv)

# Further Reading
[Compare Kotlin’s Coroutine Launch, Async-Await, or SuspendCancellableCoroutine](https://medium.com/mobile-app-development-publication/compare-kotlins-coroutine-launch-async-await-or-suspendcancellablecoroutine-a09a919f7f3b)

[What is the difference between launch/join and async/await in Kotlin coroutines](https://stackoverflow.com/questions/46226518/what-is-the-difference-between-launch-join-and-async-await-in-kotlin-coroutines)
