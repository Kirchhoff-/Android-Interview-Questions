# StateFlow
```
interface StateFlow<out T> : SharedFlow<T> (source)
```

A `SharedFlow` that represents a read-only state with a single updatable data value that emits updates to the value to its collectors. A state flow is a *hot* flow because its active instance exists independently of the presence of collectors. Its current value can be retrieved via the `value` property.

**State flow never completes**. A call to `Flow.collect` on a state flow never completes normally, and neither does a coroutine started by the `Flow.launchIn` function. An active collector of a state flow is called a *subscriber*.

A mutable state flow is created using `MutableStateFlow(value)` constructor function with the initial value. The value of mutable state flow can be updated by setting its `value` property. Updates to the `value` are always `conflated`. So a slow collector skips fast updates, but always collects the most recently emitted value.

`StateFlow` is useful as a data-model class to represent any kind of state. Derived values can be defined using various operators on the flows, with `combine` operator being especially useful to combine values from multiple state flows using arbitrary functions.

State flow is a special-purpose, high-performance, and efficient implementation of `SharedFlow` for the narrow, but widely used case of sharing a state. See the `SharedFlow` documentation for the basic rules, constraints, and operators that are applicable to all shared flows.

State flow always has an initial value, replays one most recent value to new subscribers, does not buffer any more values, but keeps the last emitted one, and does not support `resetReplayCache`. A state flow behaves identically to a shared flow when it is created with the following parameters and the `distinctUntilChanged` operator is applied to it:
```
// MutableStateFlow(initialValue) is a shared flow with the following parameters:
val shared = MutableSharedFlow(
    replay = 1,
    onBufferOverflow = BufferOverflow.DROP_OLDEST
)
shared.tryEmit(initialValue) // emit the initial value
val state = shared.distinctUntilChanged() // get StateFlow-like behavior
```

Use `SharedFlow` when you need a `StateFlow` with tweaks in its behavior such as extra buffering, replaying more values, or omitting the initial value.

All methods of state flow are thread-safe and can be safely invoked from concurrent coroutines without external synchronization.

State flow implementation is optimized for memory consumption and allocation-freedom. It uses a lock to ensure thread-safety, but suspending collector coroutines are resumed outside of this lock to avoid dead-locks when using unconfined coroutines. Adding new subscribers has `O(1)` amortized cost, but updating a `value` has `O(N)` cost, where `N` is the number of active subscribers.

Example:

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


## Links
https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-state-flow/
