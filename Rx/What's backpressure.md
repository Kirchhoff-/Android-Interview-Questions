# Backpressure

Backpressure is the inability of a Subscriber to handle all incoming events in time. In other words Backpressure can occur when the Producer of events is faster than the Consumer. There are a variety of strategies with which you can exercise flow control and backpressure in RxJava in order to alleviate the problems caused when a quickly-producing Observable meets a slow-consuming observer. 

For example, imagine using the zip operator to `zip` together two infinite Observables, one of which emits items twice as frequently as the other. A naive implementation of the `zip` operator would have to maintain an ever-expanding buffer of items emitted by the faster Observable to eventually combine with items emitted by the slower one. 

This could cause RxJava to seize an unwieldy amount of system resources. Since it requires system resources to handle backpressure, you need to choose right backpressure strategy that suits your requirement.
There are a few backpressure strategies:
- Drop: Discards the unrequested items if it exceeds the buffer size
- Buffer: Buffers all the items from the producer, watch for OOMs
- Latest: Keeps only the most recent item
- Error: throws a `MissingBackpressureException` in case of over emission
- Missing: No strategy, it would throw a MissingBackpressureException sooner or later somewhere on the downstream

## Links
https://github.com/ReactiveX/RxJava/wiki/Backpressure  
https://proandroiddev.com/rxjava-backpressure-and-why-you-should-care-369c5242c9e6  
https://www.baeldung.com/rxjava-backpressure       
