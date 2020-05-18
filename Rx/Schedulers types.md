# Schedulers

If you want to introduce multithreading into your cascade of Observable operators, you can do so by instructing those operators (or particular Observables) to operate on particular Schedulers.

By default, an `Observable` and the chain of operators that you apply to it will do its work, and will notify its observers, on the same thread on which its `subscribe()` method is called. The `subscribeOn()` operator changes this behavior by specifying a different Scheduler on which the `Observable` should operate. The `observeOn()` operator specifies a different Scheduler that the `Observable` will use to send notifications to its observers.

## Schedulers types

**IO** 
`observable.subscribeOn(Schedulers.io())`

This is one of the most common types of Schedulers that are used. Generally used for IO related stuff. Such as network requests, file system operations. IO scheduler is backed by thread-pool.

**Computation**
`observable.subscribeOn(Schedulers.computation())`

This scheduler is quite similar to IO Schedulers as this is backed by thread pool too. However, the number of threads that can be used is fixed to the number of cores present in the system. So if you have two cores in your mobile, it will have 2 threads in the pool. This also means that if these two threads are busy then the process will have to wait for them to be available.

**NewThread**
`observable.subscribeOn(Schedulers.newThread())`

Creates and returns a Scheduler that creates a new Thread for each unit of work. Since thread creation is a costly operation and can have a drastic effect in mobile environment.

**Single**
`observable.subscribeOn(Schedulers.single())`

Create single thread and perform all operations in it.

**Trampoline**
`observable.subscribeOn(Schedulers.trampoline())`

Creates and returns a Scheduler that queues work on the current thread to be executed after the current work completes. Often used in tests. 

**Executor Scheduler**
```
val executor = Executors.newFixedThreadPool(10) 
val pooledScheduler = Schedulers.from(executor)
```
Converts an Executor into a new Scheduler instance. This can be used in scenarios where number of observables could be huge for IO Schedulers

**Android scheduler** `AndroidSchedulers.mainThread()`  
This Scheduler is provided by rxAndroid library. This is used to bring back the execution to the main thread so that UI modification can be made. This is usually used in `observeOn()` method

## Links
http://reactivex.io/documentation/scheduler.html  
https://android.jlelse.eu/rxjava-schedulers-what-when-and-how-to-use-it-6cfc27293add  
https://www.baeldung.com/rxjava-schedulers  
https://www.tutorialspoint.com/rxjava/rxjava_schedulers.htm  
