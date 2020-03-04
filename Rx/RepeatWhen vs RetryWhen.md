# Repeat vs. Retry

Main difference between `repeat` and `retry` is event causes a resubscription:
- `repeat()` resubscribes when it receives `onCompleted()`.
- `retry()` resubscribes when it receives `onError()`.

Key points about `repeatWhen` and `retryWhen`: 
- `repeatWhen` is identical to `retryWhen`, only it responds to `onCompleted` instead of `onError`. The input is `Observable<Void>`, since `onCompleted` has no type.
- The `notificationHandler` (i.e. `Func1`) is only called once per subscription. This makes sense as you are given an `Observable<Throwable>`, which can emit any number of errors.
- The output `Observable` has to use the input `Observable` as its source. You must react to the `Observable<Throwable>` and emit based on it; you can't just return a generic stream.
- The `Observable` input only triggers on terminal events (`onCompleted` for `repeatWhen` / `onError` for `retryWhen`). It doesn't receive any of the `onNext` notifications from the source, so you can't examine the emitted data to determine if you should resubscribe. If you want to do that, you have to add an operator like `takeUntil()` to cut off the stream.

## Links
https://blog.danlew.net/2016/01/25/rxjavas-repeatwhen-and-retrywhen-explained/  
https://android.jlelse.eu/rx-grokking-retrywhen-and-repeatwhen-on-android-development-examples-af5c3ed0872b  
