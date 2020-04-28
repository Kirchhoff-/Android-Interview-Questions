# ObserveOn() vs SubscribeOn()

`observeOn()` - specify the Scheduler on which an observer will observe this Observable. This method simply changes the thread of all operators further downstream (in the calls that come after). Example: 
```
Observable.just("Random String")              // UI
       .map(str -> str.length())              // UI
       .observeOn(Schedulers.computation())   // Changing the thread
       .map(length -> length - 2)             // Computation
       .subscribe(result -> Log.d("", "Result = $result")
```

`subscribeOn()` - specify the Scheduler on which an Observable will operate. Position does not matter `subscribeOn()` can be put in any place in the stream because it affects only the time of subscription. Example:
```
Observable.just("Random String")              // Computation
       .map(str -> str.length())              // Computation
       .map(length -> length - 2)             // Computation
       .subscribeOn(Schedulers.computation()) 
       .subscribe(result -> Log.d("", "Result = $result") // Computation
```


![](./res/schedulers.png "Schedulers")

By default, an Observable and the chain of operators that you apply to it will do its work, and will notify its observers, on the same thread on which its Subscribe method is called. The SubscribeOn operator changes this behavior by specifying a different Scheduler on which the Observable should operate. The ObserveOn operator specifies a different Scheduler that the Observable will use to send notifications to its observers.

As shown in this illustration, the SubscribeOn operator designates which thread the Observable will begin operating on, no matter at what point in the chain of operators that operator is called. ObserveOn, on the other hand, affects the thread that the Observable will use below where that operator appears. For this reason, you may call ObserveOn multiple times at various points during the chain of Observable operators in order to change on which threads certain of those operators operate.

Conclusion:
- `observeOn()` works only downstream.
- `subscribeOn()` works downstream and upstream.
- consecutive subscribeOns do not change the thread.
- consequent observeOns do change the thread.
- Thread change by an `observeOn()` cannot be overridden by a `subscribeOn()`.

## Links
http://reactivex.io/documentation/operators/observeon.html  
http://reactivex.io/documentation/operators/subscribeon.html  
https://medium.com/upday-devs/rxjava-subscribeon-vs-observeon-9af518ded53a  
https://stackoverflow.com/questions/44984730/rxandroid-whats-the-difference-between-subscribeon-and-observeon  
https://proandroiddev.com/rx-java-subscribeon-and-observeon-a7d95041ce96
