# Combining Operators
Operators that work with multiple source Observables to create a single `Observable`.
- `CombineLatest` — when an item is emitted by either of two Observables, combine the latest item emitted by each `Observable` via a specified function and emit items based on the results of this function;
- `Join` — combine items emitted by two Observables whenever an item from one `Observable` is emitted during a time window defined according to an item emitted by the other `Observable`;
- `Merge` — combine multiple Observables into one by merging their emissions;
- `StartWith` — emit a specified sequence of items before beginning to emit the items from the source `Observable`;
- `Switch` — convert an `Observable` that emits Observables into a single `Observable` that emits the items emitted by the most-recently-emitted of those Observables;
- `Zip` — combine the emissions of multiple Observables together via a specified function and emit single items for each combination based on the results of this function.

### [CombineLatest](http://reactivex.io/documentation/operators/combinelatest.html)
When an item is emitted by either of two Observables, combine the latest item emitted by each `Observable` via a specified function and emit items based on the results of this function.

The *CombineLatest* operator behaves in a similar way to *Zip*, but while *Zip* emits items only when each of the zipped source Observables have emitted a previously unzipped item, *CombineLatest* emits an item whenever any of the source Observables emits an item (so long as each of the source Observables has emitted at least one item). When any of the source Observables emits an item, *CombineLatest* combines the most recently emitted items from each of the other source Observables, using a function you provide, and emits the return value from that function.

![](./res/combining_combineLatest.png "CombineLatest")

```
fun combineLatest() {
    val firstObservable = Observable.interval(100, TimeUnit.MILLISECONDS)
    val secondObservable = Observable.interval(50, TimeUnit.MILLISECONDS)

    Observable
            .combineLatest(firstObservable, secondObservable) { firstItem, secondItem ->
                "Item from first observable = $firstItem, item from second observable = $secondItem"
            }
            .subscribe(
                    { result -> println("Next item = $result") },
                    { println("onError") },
                    { println("onComplete") }
            )
}
```

Output:
```
Next item = Item from first observable = 0, item from second observable = 0
Next item = Item from first observable = 0, item from second observable = 1
Next item = Item from first observable = 0, item from second observable = 2
Next item = Item from first observable = 1, item from second observable = 2
Next item = Item from first observable = 1, item from second observable = 3
Next item = Item from first observable = 1, item from second observable = 4
Next item = Item from first observable = 2, item from second observable = 4
Next item = Item from first observable = 2, item from second observable = 5
Next item = Item from first observable = 2, item from second observable = 6
...
```

### [Join](http://reactivex.io/documentation/operators/join.html)
Сombine items emitted by two Observables whenever an item from one `Observable` is emitted during a time window defined according to an item emitted by the other `Observable`.

The *Join* operator combines the items emitted by two Observables, and selects which items to combine based on duration-windows that you define on a per-item basis. You implement these windows as Observables whose lifespans begin with each item emitted by either `Observable`. When such a window-defining `Observable` either emits an item or completes, the window for the item it is associated with closes. So long as an item’s window is open, it will combine with any item emitted by the other `Observable`. You define the function by which the items combine.

![](./res/combining_join.png "Join")

```
fun join() {
    val firstObservable = Observable.interval(200, TimeUnit.MILLISECONDS)
    val secondObservable = Observable.interval(200, TimeUnit.MILLISECONDS)

    firstObservable.join(
            secondObservable,
            { leftEnd -> Observable.just(leftEnd) },
            { rightEnd -> Observable.just(rightEnd) },
            { left, right -> left + right }
            )
            .subscribe(
                    { result -> println("Next item = $result") },
                    { println("onError") },
                    { println("onComplete") }
            )
}
```

Output:
```
Next item = 2
Next item = 6
Next item = 10
Next item = 14
Next item = 16
Next item = 18
Next item = 20
Next item = 22
Next item = 24
```

### [Merge](http://reactivex.io/documentation/operators/merge.html)
Combine multiple Observables into one by merging their emissions. 

You can combine the output of multiple Observables so that they act like a single `Observable`, by using the *Merge* operator.

*Merge* may interleave the items emitted by the merged Observables (a similar operator, *Concat*, does not interleave items, but emits all of each source Observable’s items in turn before beginning to emit items from the next source `Observable`).

As shown in the below diagram, an `onError` notification from any of the source Observables will immediately be passed through to observers and will terminate the merged `Observable`.

![](./res/combining_merge.png "Merge")

```
fun merge() {
    Observable.just(1, 2, 3)
            .mergeWith(Observable.just(5, 6, 7))
            .subscribe(
                    { result -> println("Next item = $result") },
                    { println("onError") },
                    { println("onComplete") }
            )
}
```

Output:
```
Next item = 1
Next item = 2
Next item = 3
Next item = 5
Next item = 6
Next item = 7
onComplete
```

### [StartWith](http://reactivex.io/documentation/operators/startwith.html)
Emit a specified sequence of items before beginning to emit the items from the source `Observable`.  

If you want an `Observable` to emit a specific sequence of items before it begins emitting the items normally expected from it, apply the *StartWith* operator to it.

(If, on the other hand, you want to append a sequence of items to the end of those normally emitted by an `Observable`, you want the *Concat* operator.)

![](./res/combining_startWith.png "StartWith")

```
fun startWith() {
    val observable = Observable.defer { Observable.just(1, 2, 3, 4, 5) }

    observable.startWith(listOf(-5, -4, -3, -2, -1, 0))
            .subscribe(
                    { result -> println("Next item = $result") },
                    { println("onError") },
                    { println("onComplete") }
            )
}
```

Output:
```
Next item = -5
Next item = -4
Next item = -3
Next item = -2
Next item = -1
Next item = 0
Next item = 1
Next item = 2
Next item = 3
Next item = 4
Next item = 5
onComplete
```

### [Switch](http://reactivex.io/documentation/operators/switch.html)
Convert an `Observable` that emits Observables into a single `Observable` that emits the items emitted by the most-recently-emitted of those Observables. 

*Switch* subscribes to an `Observable` that emits Observables. Each time it observes one of these emitted Observables, the `Observable` returned by *Switch* unsubscribes from the previously-emitted `Observable` begins emitting items from the latest `Observable`. Note that it will unsubscribe from the previously-emitted `Observable` when a new `Observable` is emitted from the source `Observable`, not when the new `Observable` emits an item. This means that items emitted by the previous `Observable` between the time the subsequent `Observable` is emitted and the time that subsequent `Observable` itself begins emitting items will be dropped (as with the yellow circle in the diagram below). RxJava implements this operator as `switchOnNext`.

![](./res/combining_switch.png "Switch")

```
fun switch() {
    Observable
            .switchOnNext(Observable.interval(600, TimeUnit.MILLISECONDS)
            .map { return@map Observable.interval(150, TimeUnit.MILLISECONDS) })
            .subscribe(
                    { result -> println("Next item = $result") },
                    { println("onError") },
                    { println("onComplete") }
            )
}
```

Output:
```
Next item = 0
Next item = 1
Next item = 2
Next item = 0
Next item = 1
Next item = 2
Next item = 0
Next item = 1
Next item = 2
```

### [Zip](http://reactivex.io/documentation/operators/zip.html)
Combine the emissions of multiple Observables together via a specified function and emit single items for each combination based on the results of this function. 

The *Zip* method returns an `Observable` that applies a function of your choosing to the combination of items emitted, in sequence, by two (or more) other Observables, with the results of this function becoming the items emitted by the returned `Observable`. It applies this function in strict sequence, so the first item emitted by the new `Observable` will be the result of the function applied to the first item emitted by `Observable` #1 and the first item emitted by `Observable` #2; the second item emitted by the new zip-`Observable` will be the result of the function applied to the second item emitted by `Observable` #1 and the second item emitted by `Observable` #2; and so forth. It will only emit as many items as the number of items emitted by the source `Observable` that emits the fewest items.

![](./res/combining_zip.png "Zip")

```
fun zip() {
    val productsObservable = Observable.just("Banana", "Orange", "Apple")
    val productsAmount = Observable.just(1, 2, 3)

    productsObservable.zipWith(productsAmount) { firstItem, secondItem -> "Product = $firstItem, amount = $secondItem"}
            .subscribe(
                    { result -> println("Next item = $result") },
                    { println("onError") },
                    { println("onComplete") }
            )
}
```

Output:
```
Next item = Product = Banana, amount = 1
Next item = Product = Orange, amount = 2
Next item = Product = Apple, amount = 3
```

# Links
http://reactivex.io/documentation/operators.html  
http://reactivex.io/documentation/operators/combinelatest.html  
http://reactivex.io/documentation/operators/join.html  
http://reactivex.io/documentation/operators/merge.html  
http://reactivex.io/documentation/operators/startwith.html  
http://reactivex.io/documentation/operators/switch.html  
http://reactivex.io/documentation/operators/zip.html
