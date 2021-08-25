# Cold vs Hot Observables

## Cold Observables

Cold observables are observables where the data producer is created by the observable itself. For example, observables created using the `of`, `from`, `range`, `interval` and `timer` operators will be cold. he data is created from within the observable itself, and there truly is not data being produced until the observable is subscribed to.

Cold Observables will replay the emissions to each `Observer`, ensuring that all Observers get all the data. Each observer will have its own set of items emitted to them and depending on how the observable was created, will have different instances of emitted items.

Example:
```
private fun coldObservables() {
   val observable = Observable.interval(1, TimeUnit.SECONDS)
   observable.subscribe { item -> println("Observer 1: $item") }
   Thread.sleep(3000)
   observable.subscribe { item -> println("Observer 2: $item") }
   Thread.sleep(3000)
}
```

Output:
```
Observer 1: 0
Observer 1: 1
Observer 1: 2
Observer 1: 3
Observer 2: 0
Observer 1: 4
Observer 2: 1
Observer 1: 5
Observer 2: 2
Observer 1: 6
Observer 2: 3
Observer 1: 7
Observer 2: 4
Observer 1: 8
Observer 2: 5
Observer 1: 9
Observer 2: 6
Observer 1: 10
Observer 2: 7
```

## Hot Observables

In contrast to cold observables, hot observables emit items regardless of whether there are observers. In a hot observable, there is a single source of emission and depending on when observers subscribe, they may miss some of those emissions. An observable is hot if its data producer is created or activated outside observable. 

Most common examples of this are UI events. For example in Android, a click event can be represented as an observable. And it is only logical for observers to receive only the events after they subscribed, so the click observable has no need to cache them for replay.

`PublishSubject` is example of hot observable.

Example:
```
private fun hotObservables() {
  val subject = PublishSubject.create<Int>()
  subject.onNext(0)

  subject.subscribe { item -> println("Observer 1: $item") }

  subject.onNext(1)
  subject.onNext(2)

  subject.subscribe { item -> println("Observer 2: $item") }

  subject.onNext(3)
  subject.onNext(4)
}
```

Output:
```
Observer 1: 1
Observer 1: 2
Observer 1: 3
Observer 2: 3
Observer 1: 4
Observer 2: 4
```

## Conclusion
**Cold Observable**:
- A cold Observable begins emitting elements only when it is subscribed to;
- A subscriber of a cold Observable is guaranteed to see the whole sequence from the beginning.

**Hot Observable**:
- A hot Observable begins emitting elements immediately and before it is subscribed to;
- Any observer who later subscribes to a hot Observable may start observing the sequence anywhere in the middle.

# Links
[RxJS: Hot And Cold Observables](https://alligator.io/rxjs/hot-cold-observables/)

[RxJava: Hot vs Cold Observable](https://mouaad.aallam.com/rxjava-hot-vs-cold-observable/)

[RxJava Ninja: Hot and Cold Observables](https://medium.com/tompee/rxjava-ninja-hot-and-cold-observables-19b30d6cc2fa)

[Hot vs Cold Observables](https://www.stepintoswift.com/rxswift-hot-vs-cold-obserables)

# Further reading
[Exploring RxJava in Android — Different types of Subjects](https://proandroiddev.com/rxjava-different-types-of-subjects-ef9183b5e87e)