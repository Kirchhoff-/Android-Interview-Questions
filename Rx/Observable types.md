# Observable types

Observables are the source which emits items to the Observers. Observables are the sources for the data. Usually they start providing data once a subscriber starts listening. An observable may emit any number of items (including zero items). It can terminate either successfully or with an error.

## Observable
Emits 0 or n items and terminates with an success or an error event. The Observable class is the non-backpressured, optionally multi-valued base reactive class that offers factory methods, intermediate operators and the ability to consume synchronous and/or asynchronous reactive dataflows.

## Flowable
Emits 0 or n items and terminates with an success or an error event. Supports backpressure, which allows to control how fast a source emits items. The Flowable class that implements the Reactive Streams Pattern and offers factory methods, intermediate operators and the ability to consume reactive dataflows.

## Single
Emits either a single item or an error event. The reactive version of a method call. there are 2 methods in the subscription they are all mutually exclusive so just one of them can be called at the end. To know: 
- **onError** - will be called whenever an error is thrown in some point of the stream.
- **onSuccess** - will be called if we get a response from the stream, in this case information about the user and we use a `Single` because we know 100% that a user has information to show.

## Maybe
Succeeds with an item, or no item, or errors. The reactive version of an Optional. Maybe emits 0 or 1 items, but it can complete without emitting a value. There are 3 methods in the subscription they are all mutually exclusive so just one of them can be called at the end:
- **onError** - will be called whenever an error is thrown in some point of the stream.
- **onSuccess** - will be called if we get a response from the stream.
- **onComplete** - will be called if we have an empty output.

## Completable
Either completes with an success or with an error event. It never emits items. The reactive version of a Runnable. Completable is used in cases where you need to know whether an operation is completable successfully or not. Unlike Maybe and Single, the CompletableObserver doesn’t return any value at all. There are two method for subscription: 
- **onError** - will be called whenever an error is thrown at some point of the stream or if, in this case, we couldn’t perform the login for some reason.
- **onComplete** - will be called if everything went right. In this case we don’t have any output, just a code block that will be invoked afterwards.

## Links
http://reactivex.io/documentation/observable.html  
https://www.journaldev.com/22594/rxjava-observables-observers  
https://bugfender.com/blog/data-flows-in-rxjava2-observable-flowable-single-maybe-completable/   
https://www.vogella.com/tutorials/RxJava/article.html
