# Filtering Operators
Operators that selectively emit items from a source `Observable`.  
`Debounce` — only emit an item from an `Observable` if a particular timespan has passed without it emitting another item  
`Distinct` — suppress duplicate items emitted by an `Observable`  
`ElementAt` — emit only item `n` emitted by an `Observable`  
`Filter` — emit only those items from an `Observable` that pass a predicate test  
`First` — emit only the first item, or the first item that meets a condition, from an `Observable`  
`IgnoreElements` — do not emit any items from an `Observable` but mirror its termination notification  
`Last` — emit only the last item emitted by an `Observable`  
`Sample` — emit the most recent item emitted by an `Observable` within periodic time intervals  
`Skip` — suppress the first `n` items emitted by an `Observable`  
`SkipLast` — suppress the last `n` items emitted by an `Observable`  
`Take` — emit only the first `n` items emitted by an `Observable`  
`TakeLast` — emit only the last `n` items emitted by an `Observable`  

### [Debounce](http://reactivex.io/documentation/operators/debounce.html)
Only emit an item from an `Observable` if a particular timespan has passed without it emitting another item. The *Debounce* operator filters out items emitted by the source `Observable` that are rapidly followed by another emitted item

![](./res/filtering_debounce.png "Debounce")

```
fun debounce() {
    val observable = Observable.create<Int> { emitter ->
        run {
            for (i in 0..5) {
                Thread.sleep(1000)
                emitter.onNext(i)
            }

            emitter.onComplete()
        }
    }

    observable.debounce(1500, TimeUnit.MILLISECONDS)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 5
```

### [Distinct](http://reactivex.io/documentation/operators/distinct.html)
Suppress duplicate items emitted by an `Observable`. The *Distinct* operator filters an` Observable` by only allowing items through that have not already been emitted.

![](./res/filtering_distinct.png "Distinct")

```
fun distinct() {
    Observable.just(1, 2, 1, 1, 2, 3)
            .distinct()
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 1
Next item = 2
Next item = 3
```

### [ElementAt](http://reactivex.io/documentation/operators/elementat.html)
Emit only item `n` emitted by an `Observable`. The *ElementAt* operator pulls an item located at a specified index location in the sequence of items emitted by the source `Observable` and emits that item as its own sole emission.

![](./res/filtering_elementAt.png "ElementAt")

```
fun elementAt() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8, 9)
            .elementAt(7)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 8
```

### [Filter](http://reactivex.io/documentation/operators/filter.html)
Emit only those items from an `Observable` that pass a predicate test. The *Filter* operator filters an `Observable` by only allowing items through that pass a test that you specify in the form of a predicate function.

![](./res/filtering_filter.png "Filter")

```
fun filter() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .filter { it % 2 == 0 }
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 2
Next item = 4
Next item = 6
Next item = 8
```

### [First](http://reactivex.io/documentation/operators/first.html)
Emit only the first item (or the first item that meets some condition) emitted by an `Observable`. If you are only interested in the first item emitted by an `Observable`, or the first item that meets some criteria, you can filter the `Observable` with the *First* operator.

![](./res/filtering_first.png "First")

```
fun first() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .first(0)
            .subscribe { result -> println("Next item (first() for not empty observable) = $result") }

    Observable.just(8, 7, 6, 5, 4, 3, 2, 1)
            .firstElement()
            .subscribe { result -> println("Next item (firstElement()) = $result") }

    Observable.empty<Int>()
            .first(0)
            .subscribe { result -> println("Next item (first() for empty observable) = $result") }
}
```

Output: 
```
Next item (first() for not empty observable) = 1
Next item (firstElement()) = 8
Next item (first() for empty observable) = 0
```

### [IgnoreElements](http://reactivex.io/documentation/operators/ignoreelements.html)
Do not emit any items from an `Observable` but mirror its termination notification. The *IgnoreElements* operator suppresses all of the items emitted by the source `Observable`, but allows its termination notification (either `onError` or `onCompleted`) to pass through unchanged.

![](./res/filtering_ignoreElements.png "IgnoreElements")

```
fun ignoreElements() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .ignoreElements()
            .subscribe(object : CompletableObserver {
                override fun onComplete() {
                    println("onComplete")
                }

                override fun onSubscribe(d: Disposable) {
                    println("onSubscribed")
                }

                override fun onError(e: Throwable) {}
            })
}
```

Output:
```
onSubscribed
onComplete
```

### [Last](http://reactivex.io/documentation/operators/last.html)
Emit only the last item (or the last item that meets some condition) emitted by an `Observable`. If you are only interested in the last item emitted by an `Observable`, or the last item that meets some criteria, you can filter the `Observable` with the Last operator.

![](./res/filtering_last.png "Last")

```
fun last() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .last(0)
            .subscribe { result -> println("Next item (last() for not empty observable) = $result") }

    Observable.just(8, 7, 6, 5, 4, 3, 2, 1)
            .lastElement()
            .subscribe { result -> println("Next item (lastElement()) = $result") }

    Observable.empty<Int>()
            .last(0)
            .subscribe { result -> println("Next item (last() for empty observable) = $result") }
}
```

Output:
```
Next item (last() for not empty observable) = 8
Next item (lastElement()) = 1
Next item (last() for empty observable) = 0
```

### [Sample](http://reactivex.io/documentation/operators/sample.html)
Emit the most recent items emitted by an `Observable` within periodic time intervals. The *Sample* operator periodically looks at an `Observable` and emits whichever item it has most recently emitted since the previous sampling.

![](./res/filtering_sample.png "Sample")

```
fun sample() {
    val observable = Observable.create<Int> { emitter ->
        run {
            for (i in 0..5) {
                Thread.sleep(1000)
                emitter.onNext(i)
            }

            emitter.onComplete()
        }
    }

    observable.sample(1500, TimeUnit.MILLISECONDS)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 0
Next item = 1
Next item = 3
Next item = 4
```

### [Skip](http://reactivex.io/documentation/operators/skip.html)
Suppress the first `n` items emitted by an `Observable`. You can ignore the first `n` items emitted by an `Observable` and attend only to those items that come after, by modifying the `Observable` with the *Skip* operator.

![](./res/filtering_skip.png "Skip")

```
fun skip() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .skip(5)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 6
Next item = 7
Next item = 8
```

### [SkipLast](http://reactivex.io/documentation/operators/skiplast.html)
Suppress the final `n` items emitted by an `Observable`. You can ignore the final `n` items emitted by an `Observable` and attend only to those items that come before them, by modifying the `Observable` with the *SkipLast* operator.

![](./res/filtering_skipLast.png "SkipLast")

```
fun skipLast() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .skipLast(5)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 1
Next item = 2
Next item = 3
```

### [Take](http://reactivex.io/documentation/operators/take.html)
Emit only the first `n` items emitted by an `Observable`. You can emit only the first `n` items emitted by an `Observable` and then complete while ignoring the remainder, by modifying the `Observable` with the *Take* operator.

![](./res/filtering_take.png "Take")

```
fun take() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .take(5)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 1
Next item = 2
Next item = 3
Next item = 4
Next item = 5
```

### [TakeLast](http://reactivex.io/documentation/operators/takelast.html)
Emit only the final `n` items emitted by an `Observable`. You can emit only the final `n` items emitted by an `Observable` and ignore those items that come before them, by modifying the `Observable` with the *TakeLast* operator.

![](./res/filtering_takeLast.png "TakeLast")

```
fun takeLast() {
    Observable.just(1, 2, 3, 4, 5, 6, 7, 8)
            .takeLast(3)
            .subscribe { result -> println("Next item = $result") }
}
```

Output:
```
Next item = 6
Next item = 7
Next item = 8
```

# Links
http://reactivex.io/documentation/operators.html  
http://reactivex.io/documentation/operators/debounce.html  
http://reactivex.io/documentation/operators/distinct.html  
http://reactivex.io/documentation/operators/elementat.html  
http://reactivex.io/documentation/operators/filter.html  
http://reactivex.io/documentation/operators/first.html  
http://reactivex.io/documentation/operators/ignoreelements.html  
http://reactivex.io/documentation/operators/last.html  
http://reactivex.io/documentation/operators/sample.html  
http://reactivex.io/documentation/operators/skip.html  
http://reactivex.io/documentation/operators/skiplast.html  
http://reactivex.io/documentation/operators/take.html  
http://reactivex.io/documentation/operators/takelast.html
