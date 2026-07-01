# How to transform COLD observables to HOT and vice versa?

A cold `Observable` starts emitting items only when a subscriber subscribes to it. Each new subscriber gets its own independent execution of the stream.

A hot `Observable` emits items regardless of whether there are active subscribers. Subscribers only receive items emitted after they subscribe.

## Transform Cold Observable to Hot Observable

A cold observable can be transformed into a hot one when multiple subscribers need to share the same source execution instead of creating a new execution for every subscription.

RxJava provides several ways to achieve this:

- `publish().refCount()` - shares the source between subscribers and automatically connects when the first subscriber appears. When all subscribers unsubscribe, the connection is disposed.
- `share()` - a shorter version of `publish().refCount()`. It is usually used when we only need simple source sharing.
- `replay().refCount()` - shares the source and replays emitted items to new subscribers. It is useful when late subscribers should receive previous values.
- `publish().autoConnect()` - starts the source after a required number of subscribers and keeps it connected even if subscribers unsubscribe.
- `cache()` - subscribes to the source once, stores all emitted items, and replays them to future subscribers. It should be used carefully because it can keep many items in memory.

Example with `publish().refCount()`:

```kotlin
private fun coldToHotObservable() {
    val coldObservable = Observable.interval(1, TimeUnit.SECONDS)

    val hotObservable = coldObservable
        .publish()
        .refCount()

    hotObservable.subscribe { println("Observer 1: $it") }

    Thread.sleep(3000)

    hotObservable.subscribe { println("Observer 2: $it") }

    Thread.sleep(3000)
}
```

Output:

```text
Observer 1: 0
Observer 1: 1
Observer 1: 2
Observer 1: 3
Observer 2: 3
Observer 1: 4
Observer 2: 4
Observer 1: 5
Observer 2: 5
```

Both observers share the same execution of the source. Since the second observer subscribes later, it only receives items emitted after its subscription.

## Transform Hot Observable to Cold Observable

Unlike converting a cold observable into a hot one, RxJava does not provide an operator that converts a hot observable into a truly cold one.

This is because a hot observable represents an already existing event source. Once an event has been emitted, it cannot be replayed unless the source itself supports buffering or replaying values.

Instead, the recommended approach is to create a new cold observable that performs the required work for every subscriber.

For example:

```kotlin
private fun loadUser(): Single<User> {
    return Single.fromCallable {
        api.getUser()
    }
}
```

or

```kotlin
private fun loadUser(): Observable<User> {
    return Observable.create { emitter ->
        emitter.onNext(api.getUser())
        emitter.onComplete()
    }
}
```

Each subscription executes the source independently:

```kotlin
loadUser().subscribe { println("Observer 1") }

loadUser().subscribe { println("Observer 2") }
```

Output:

```text
API request
Observer 1

API request
Observer 2
```

Unlike a hot observable, every subscriber receives its own independent execution of the source.

# Related questions
[What is the difference between cold and hot observables?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Rx/What%20is%20the%20difference%20between%20cold%20and%20hot%20observables.md)
