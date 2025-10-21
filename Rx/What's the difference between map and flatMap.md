# `map` vs `flatMap`

## [`map`](https://reactivex.io/documentation/operators/map.html)
Transform the items emitted by an `Observable` by applying a function to each item. The *Map* operator applies a function of your choosing to each item emitted by the source `Observable`, and returns an `Observable` that emits the results of these function applications.

![](./res/transforming_map.png "Map")

**Example**. Suppose we have the class `Point` which contains two values - `x` and `y`, and we want to display all `x` values from some amount of points: 
```
data class Point(val x: Int = Random.nextInt(), val y: Int = Random.nextInt())

fun mapOperatorExample() {
  Observable.just(
    Point(),
    Point(),
    Point(),
  ) //Observable<Point>
    .map { it.x } //After applying `map()` function Observable<Point> transforms to Observable<Int>
    .subscribe { print(it) }
}
```

## [`flatMap`](https://reactivex.io/documentation/operators/flatmap.html)
Transform the items emitted by an `Observable` into Observables, then flatten the emissions from those into a single `Observable`.

The *FlatMap* operator transforms an `Observable` by applying a function that you specify to each item emitted by the source `Observable`, where that function returns an `Observable` that itself emits items. *FlatMap* then merges the emissions of these resulting Observables, emitting these merged results as its own sequence.

![](./res/transforming_flatMap.png "FlatMap")

**Example**. Suppose we need to transform each element of initial `Observable` to it's own `Observable`: 

```
fun flatMapExample() {
    Observable.just("A", "B", "C")
            .flatMap { item ->
                println()
                Observable.just(item + "1 " + item + "2 " + item + "3")
            }
            .subscribe { items -> println("Next item = $items") }
}
```

Output:
```
Next item = A1 A2 A3
Next item = B1 B2 B3
Next item = C1 C2 C3
```

## [Difference](https://stackoverflow.com/questions/22847105/when-do-you-use-map-vs-flatmap-in-rxjava)
- `map` transform one event to another, `flatMap` transform one event to zero or more event;
- `map` emits single element and `flatMap` emits a stream of elements;
- `map` maps items to items, `flatmap` maps observables to observables;
- `map` recommended for transformation the event, `flatMap` recommended if you plan to make an asynchronous call inside the method.

# Links
[Map](https://reactivex.io/documentation/operators/map.html)

[FlatMap](https://reactivex.io/documentation/operators/flatmap.html)

[When do you use map vs flatMap in RxJava?](https://stackoverflow.com/questions/22847105/when-do-you-use-map-vs-flatmap-in-rxjava)

# Next questions
[Can you provide an example of transforming operators?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Rx/Give%20example%20of%20transforming%20operators.md)
