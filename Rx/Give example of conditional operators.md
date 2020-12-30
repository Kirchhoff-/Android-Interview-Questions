# Conditional and Boolean Operators
Operators that evaluate one or more Observables or items emitted by Observables:  
`All` — determine whether all items emitted by an `Observable` meet some criteria  
`Amb` — given two or more source Observables, emit all of the items from only the first of these Observables to emit an item  
`Contains` — determine whether an `Observable` emits a particular item or not  
`DefaultIfEmpty` — emit items from the source `Observable`, or a default item if the source `Observable` emits nothing  
`SequenceEqual` — determine whether two Observables emit the same sequence of items  
`SkipUntil` — discard items emitted by an `Observable` until a second `Observable` emits an item  
`SkipWhile` — discard items emitted by an `Observable` until a specified condition becomes false  
`TakeUntil` — discard items emitted by an `Observable` after a second `Observable` emits an item or terminates  
`TakeWhile` — discard items emitted by an `Observable` after a specified condition becomes false  

### All

Pass a predicate function to the `All` operator that accepts an item emitted by the source `Observable` and returns a `boolean` value based on an evaluation of that item. All returns an `Observable` that emits a single `boolean` value: `true` if and only if the source `Observable` terminates normally and every item emitted by the source `Observable` evaluated as `true` according to this predicate; `false` if any item emitted by the source `Observable` evaluates as false according to this predicate.

![](./res/conditional_all.png "All")

```
private fun all() {
    Observable.just(1, 2, 3)
            .all { number -> number % 2 == 0 }
            .subscribe { result -> println("Is all numbers even = $result") }

    Observable.just(2, 4, 8)
            .all { number -> number % 2 == 0 }
            .subscribe { result -> println("Is all numbers even = $result") }
}
```

Output:
```
All numbers even = false
All numbers even = true
```

### Amb
Given two or more source Observables, emit all of the items from only the first of these Observables to emit an item or notification.

When you pass a number of source Observables to `Amb`, it will pass through the emissions and notifications of exactly one of these Observables: the first one that sends a notification to `Amb`, either by emitting an item or sending an `onError` or `onCompleted` notification. Amb will ignore and discard the emissions and notifications of all of the other source Observables.

![](./res/conditional_amb.png "amb")

```
private fun amb() {
    val firstObservable = Observable.timer(4, TimeUnit.SECONDS)
            .flatMap { Observable.just(1, 3, 5, 7, 9, 11, 13, 15) }

    val secondObservable = Observable.timer(3, TimeUnit.SECONDS)
            .flatMap { Observable.just(2, 4, 6, 8, 10, 12, 14) }

    Observable
            .amb(listOf(firstObservable, secondObservable))
            .subscribe { result -> println("Next item = $result") }
}
```
`secondObservable` gets executed and the `firstObservable` is ignored

Output:
```
Next item = 2
Next item = 4
Next item = 6
Next item = 8
Next item = 10
Next item = 12
Next item = 14
```

### Contains
Determine whether an `Observable` emits a particular item or not. Pass the `Contains` operator a particular item, and the `Observable` it returns will emit `true` if that item is emitted by the source `Observable`, or `false` if the source `Observable` terminates without emitting that item.

![](./res/conditional_contains.png "Contains")

```
fun contains() {
    Observable.just(1, 2, 3, 10, 11, 12)
            .contains(100)
            .subscribe { result -> println("Sequence contains element = $result") }

    Observable.just(1, 2, 3, 10, 11, 12)
            .contains(12)
            .subscribe { result -> println("Sequence contains element = $result") }
}
```

Output:
```
Sequence contains element = false
Sequence contains element = true
```

### DefaultIfEmpty
Emit items from the source `Observable`, or a default item if the source `Observable` emits nothing. The `DefaultIfEmpty` operator simply mirrors the source `Observable` exactly if the source `Observable` emits any items. If the source `Observable` terminates normally (with an `onComplete`) without emitting any items, the `Observable` returned from `DefaultIfEmpty` will instead emit a default item of your choosing before it too completes.

![](./res/conditional_defaultIfEmpty.png "defaultIfEmpty")

```
fun defaultIfEmpty() {
    Observable.empty<Any>()
            .defaultIfEmpty(10)
            .subscribe { result: Any -> println("Default if empty = $result") }
}
```

Output:
```
Default if empty = 10
```

## SequenceEqual
Determine whether two Observables emit the same sequence of items. Pass `SequenceEqual` two Observables, and it will compare the items emitted by each `Observable`, and the `Observable` it returns will emit `true` only if both sequences are the same (the same items, in the same order, with the same termination state).

![](./res/conditional_sequenceEqual.png "sequenceEqual")

```
fun sequenceEquals() {
    Observable.sequenceEqual(Observable.just(1, 2, 3), Observable.just(3, 2, 1))
            .subscribe { result -> println("Sequence equal = $result") }

    Observable.sequenceEqual(Observable.just(1, 2, 3), Observable.just(1, 2, 3))
            .subscribe { result -> println("Sequence equal = $result") }
}
```

Output:
```
Sequence equal = false
Sequence equal = true
```

### SkipUntil
Discard items emitted by an `Observable` until a second `Observable` emits an item. The `SkipUntil` subscribes to the source `Observable`, but ignores its emissions until such time as a second `Observable` emits an item, at which point `SkipUntil` begins to mirror the source `Observable`.

![](./res/conditional_skipUntil.png "skipUntil")

```
fun skipUntil() {
    val firstObservable = Observable.create<Int>{ emitter ->
        run {
            for (i in 0..5) {
                Thread.sleep(1000)
                emitter.onNext(i)
            }

            emitter.onComplete()
        }
    }

    val secondObservable = Observable.timer(3, TimeUnit.SECONDS)
            .map { Observable.just(10, 11, 12, 13, 14, 15) }

    firstObservable.skipUntil(secondObservable)
            .subscribe { result -> println("Next item = $result") }
}
```
`firstObservable` starts emitting items after 3 seconds

Output:
```
Next item = 3
Next item = 4
Next item = 5
```

### SkipWhile
Discard items emitted by an `Observable` until a specified condition becomes `false`. The `SkipWhile` subscribes to the source `Observable`, but ignores its emissions until such time as some condition you specify becomes `false`, at which point `SkipWhile` begins to mirror the source `Observable`.

![](./res/conditional_skipWhile.png "skipWhile")

```
fun skipWhile() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8, 9)
            .skipWhile { number -> number < 5 }
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 5
Next item = 6
Next item = 7
Next item = 8
Next item = 9
```

### TakeUntil
Discard any items emitted by an `Observable` after a second `Observable` emits an item or terminates. The `TakeUntil` subscribes and begins mirroring the source `Observable`. It also monitors a second `Observable` that you provide. If this second `Observable` emits an item or sends a termination notification, the `Observable` returned by `TakeUntil` stops mirroring the source `Observable` and terminates.

![](./res/conditional_takeUntil.png "takeUntil")

```
fun takeUntil() {
    val firstObservable = Observable.create<Int>{ emitter ->
        run {
            for (i in 0..5) {
                Thread.sleep(1000)
                emitter.onNext(i)
            }

            emitter.onComplete()
        }
    }

    val secondObservable = Observable.timer(2, TimeUnit.SECONDS)
            .map { Observable.just(10, 11, 12, 13, 14, 15) }

    firstObservable.takeUntil(secondObservable)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 0
Next item = 1
```

### TakeWhile
Mirror items emitted by an `Observabl`e until a specified condition becomes `false`. The `TakeWhile` mirrors the source `Observable` until such time as some condition you specify becomes `false`, at which point `TakeWhile` stops mirroring the source `Observable` and terminates its own `Observable`.

![](./res/conditional_takeWhile.png "takeWhile")

```
fun takeWhile() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8, 9)
            .takeWhile { number -> number < 5 }
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 1
Next item = 2
Next item = 3
Next item = 4
```

# Links
http://reactivex.io/documentation/operators.html  
http://reactivex.io/documentation/operators/all.html  
http://reactivex.io/documentation/operators/amb.html  
http://reactivex.io/documentation/operators/contains.html  
http://reactivex.io/documentation/operators/defaultifempty.html  
http://reactivex.io/documentation/operators/sequenceequal.html  
http://reactivex.io/documentation/operators/skipuntil.html  
http://reactivex.io/documentation/operators/skipwhile.html  
http://reactivex.io/documentation/operators/takeuntil.html  
http://reactivex.io/documentation/operators/takewhile.html  
https://proandroiddev.com/exploring-rxjava-in-android-conditional-and-boolean-operators-3bca84c773af
