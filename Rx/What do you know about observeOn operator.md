# ObserveOn
`ObserveOn` specify the Scheduler on which an observer will observe this `Observable`.

![](./res/observeOn.png "ObserveOn")

Many implementations of ReactiveX use “Schedulers” to govern an Observable’s transitions between threads in a multi-threaded environment. You can instruct an `Observable` to send its notifications to observers on a particular Scheduler by means of the `ObserveOn` operator.

![](./res/observeOn_flow.png "ObserveOn")

Note that ObserveOn will forward an `onError` termination notification immediately if it receives one, and will not wait for a slow-consuming observer to receive any not-yet-emitted items that it is aware of first. This may mean that the `onError` notification jumps ahead of (and swallows) items emitted by the source `Observable`, as in the diagram above.

The `SubscribeOn` operator is similar, but it instructs the `Observable` to itself operate on the specified Scheduler, as well as notifying its observers on that Scheduler.

By default, an `Observable` and the chain of operators that you apply to it will do its work, and will notify its observers, on the same thread on which its `Subscribe` method is called. The `SubscribeOn` operator changes this behavior by specifying a different Scheduler on which the `Observable` should operate. The `ObserveOn` operator specifies a different Scheduler that the `Observable` will use to send notifications to its observers.

As shown in this illustration, the SubscribeOn operator designates which thread the Observable will begin operating on, no matter at what point in the chain of operators that operator is called. ObserveOn, on the other hand, affects the thread that the Observable will use *below* where that operator appears. For this reason, you may call ObserveOn multiple times at various points during the chain of Observable operators in order to change on which threads certain of those operators operate.

Example
```
fun observeOn() {
       Observable.create<Int> {
           it.onNext(1)
           it.onComplete()
       }
           .observeOn(Schedulers.newThread())
           .subscribe { println("Observe on ${Thread.currentThread().name}") }
}
```

Output:
```
Observe on RxNewThreadScheduler-1
```

# Links
[ObserveOn](https://reactivex.io/documentation/operators/observeon.html)