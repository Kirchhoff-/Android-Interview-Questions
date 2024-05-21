# StateFlow and SharedFlow
`StateFlow` and `SharedFlow` are [Flow APIs](https://developer.android.com/kotlin/flow) that enable flows to optimally emit state updates and emit values to multiple consumers.

## [`StateFlow`](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow#stateflow)
`StateFlow` is a state-holder observable flow that emits the current and new state updates to its collectors. The current state value can also be read through its `value` property. To update state and send it to the flow, assign a new value to the `value` property of the `MutableStateFlow` class.

In Android, `StateFlow` is a great fit for classes that need to maintain an observable mutable state.

Following the examples from [Kotlin flows](https://developer.android.com/kotlin/flow), a `StateFlow` can be exposed from the `LatestNewsViewModel` so that the `View` can listen for UI state updates and inherently make the screen state survive configuration changes.

```
class LatestNewsViewModel(
    private val newsRepository: NewsRepository
) : ViewModel() {

    // Backing property to avoid state updates from other classes
    private val _uiState = MutableStateFlow(LatestNewsUiState.Success(emptyList()))
    // The UI collects from this StateFlow to get its state updates
    val uiState: StateFlow<LatestNewsUiState> = _uiState

    init {
        viewModelScope.launch {
            newsRepository.favoriteLatestNews
                // Update View with the latest favorite news
                // Writes to the value property of MutableStateFlow,
                // adding a new element to the flow and updating all
                // of its collectors
                .collect { favoriteNews ->
                    _uiState.value = LatestNewsUiState.Success(favoriteNews)
                }
        }
    }
}

// Represents different states for the LatestNews screen
sealed class LatestNewsUiState {
    data class Success(news: List<ArticleHeadline>): LatestNewsUiState()
    data class Error(exception: Throwable): LatestNewsUiState()
}
```

The class responsible for updating a `MutableStateFlow` is the producer, and all classes collecting from the `StateFlow` are the consumers. Unlike a cold flow built using the flow builder, a `StateFlow` is hot: collecting from the flow doesn't trigger any producer code. A `StateFlow` is always active and in memory, and it becomes eligible for garbage collection only when there are no other references to it from a garbage collection root.

To convert any flow to a `StateFlow`, use the [`stateIn`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/state-in.html) intermediate operator.

`StateFlow` and `LiveData` have similarities. Both are observable data holder classes, and both follow a similar pattern when used in your app architecture.

Note, however, that StateFlow and LiveData do behave differently:
- `StateFlow` requires an initial state to be passed in to the constructor, while `LiveData` does not.
- `LiveData.observe()` automatically unregisters the consumer when the view goes to the `STOPPED` state, whereas collecting from a `StateFlow` or any other flow does not.

## [`SharedFlow`](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow#sharedflow)
The `shareIn` function returns a `SharedFlow`, a hot flow that emits values to all consumers that collect from it. A `SharedFlow` is a highly-configurable generalization of `StateFlow`.

You can create a `SharedFlow` without using `shareIn`. As an example, you could use a `SharedFlow` to send ticks to the rest of the app so that all the content refreshes periodically at the same time. Apart from fetching the latest news, you might also want to refresh the user information section with its favorite topics collection. In the following code snippet, a `TickHandler` exposes a `SharedFlow` so that other classes know when to refresh its content. As with `StateFlow`, use a backing property of type `MutableSharedFlow` in a class to send items to the flow:

```
// Class that centralizes when the content of the app needs to be refreshed
class TickHandler(
    private val externalScope: CoroutineScope,
    private val tickIntervalMs: Long = 5000
) {
    // Backing property to avoid flow emissions from other classes
    private val _tickFlow = MutableSharedFlow<Unit>(replay = 0)
    val tickFlow: SharedFlow<Event<String>> = _tickFlow

    init {
        externalScope.launch {
            while(true) {
                _tickFlow.emit(Unit)
                delay(tickIntervalMs)
            }
        }
    }
}

class NewsRepository(
    ...,
    private val tickHandler: TickHandler,
    private val externalScope: CoroutineScope
) {
    init {
        externalScope.launch {
            // Listen for tick updates
            tickHandler.tickFlow.collect {
                refreshLatestNews()
            }
        }
    }

    suspend fun refreshLatestNews() { ... }
    ...
}
```

You can customize the `SharedFlow` behavior in the following ways:
- `replay` lets you resend a number of previously-emitted values for new subscribers.
- `onBufferOverflow` lets you specify a policy for when the buffer is full of items to be sent. The default value is `BufferOverflow.SUSPEND`, which makes the caller suspend. Other options are `DROP_LATEST` or `DROP_OLDEST`.

`MutableSharedFlow` also has a `subscriptionCount` property that contains the number of active collectors so that you can optimize your business logic accordingly. `MutableSharedFlow` also contains a `resetReplayCache` function if you don't want to replay the latest information sent to the flow.

### Example

Following class encapsulates an integer state and increments its value on each call to `inc`:
```
class CounterModel {
    private val _counter = MutableStateFlow(0) // private mutable state flow
    val counter = _counter.asStateFlow() // publicly exposed as read-only state flow

    fun inc() {
        _counter.value++
    }
}
```

Having two instances of the above `CounterModel` class one can define the sum of their counters like this:
```
val aModel = CounterModel()
val bModel = CounterModel()
val sumFlow: Flow<Int> = aModel.counter.combine(bModel.counter) { a, b -> a + b }
```

# Links
[StateFlow and SharedFlow](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow)

# Further reading
[Flow](https://kotlinlang.org/docs/reference/coroutines/flow.html)  

[Introducing StateFlow and SharedFlow](https://blog.jetbrains.com/kotlin/2020/10/kotlinx-coroutines-1-4-0-introducing-stateflow-and-sharedflow/)  

[StateFlow, End of LiveData?](https://medium.com/scalereal/stateflow-end-of-livedata-a473094229b3)

[Kotlin SharedFlow or: How I learned to stop using RxJava and love the Flow](https://proandroiddev.com/kotlin-sharedflow-or-how-i-learned-to-stop-using-rxjava-and-love-the-flow-e1b59d211715)

[StateFlow and SharedFlow: the new hot stream APIs in town](https://www.rockandnull.com/stateflow-kotlin/)

[SharedFlow vs. StateFlow: Best Practices and Real-world examples](https://medium.com/@mortitech/sharedflow-vs-stateflow-a-comprehensive-guide-to-kotlin-flows-503576b4de31)