# What tools for multithreading do you know

## [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask)
`AsyncTask` was intended to enable proper and easy use of the UI thread. However, the most common use case was for integrating into UI, and that would cause Context leaks, missed callbacks, or crashes on configuration changes. It also has inconsistent behavior on different versions of the platform, swallows exceptions from `doInBackground`, and does not provide much utility over using `Executors` directly.

`AsyncTask` is designed to be a helper class around `Thread` and `Handler` and does not constitute a generic threading framework. `AsyncTasks` should ideally be used for short operations (a few seconds at the most.) If you need to keep threads running for long periods of time, it is highly recommended you use the various APIs provided by the `java.util.concurrent` package such as `Executor`, `ThreadPoolExecutor` and `FutureTask`.

An asynchronous task is defined by a computation that runs on a background thread and whose result is published on the UI thread. An asynchronous task is defined by 3 generic types, called `Params`, `Progress` and `Result`, and 4 steps, called `onPreExecute`, `doInBackground`, `onProgressUpdate` and `onPostExecute`. <sup>[1](https://developer.android.com/reference/android/os/AsyncTask#:~:text=AsyncTask%20was%20intended,and%20onPostExecute.)</sup>

## [WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager)
`WorkManager` is an API that makes it easy to schedule deferrable, asynchronous tasks that are expected to run even if the app exits or the device restarts. The `WorkManager` API is a suitable and recommended replacement for all previous Android background scheduling APIs, including `FirebaseJobDispatcher`, `GcmNetworkManager`, and `Job Scheduler`. `WorkManager` incorporates the features of its predecessors in a modern, consistent API that works back to API level 14 while also being conscious of battery life.

`WorkManager` handles background work that needs to run when various constraints are met, regardless of whether the application process is alive or not. Background work can be started when the app is in the background, when the app is in the foreground, or when the app starts in the foreground but goes to the background. Regardless of what the application is doing, background work should continue to execute, or be restarted if Android kills its process.

## RxJava/RxAndroid
RxJava is a Java VM implementation of [Reactive Extensions](https://reactivex.io/): a library for composing asynchronous and event-based programs by using observable sequences.

It extends the [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern) to support sequences of data/events and adds operators that allow you to compose sequences together declaratively while abstracting away concerns about things like low-level threading, synchronization, thread-safety and concurrent data structures.

RxAndroid is an extension of RxJava for Android which is used only in Android application.

RxAndroid introduced the **Main Thread** required for Android.

To work with the multithreading in Android, we will need the `Looper` and `Handler` for Main Thread execution.

RxAndroid provides `AndroidSchedulers.mainThread()` which returns a scheduler and that helps in performing the task on the main UI thread that is mainly used in the Android project. So, here `AndroidSchedulers.mainThread()` is used to provide us access to the main thread of the application to perform actions like updating the UI. <sup>[2](https://blog.mindorks.com/rxjava-for-android-rxandroid/#:~:text=RxAndroid%20is%20an,updating%20the%20UI.)</sup>

## Coroutines
A *coroutine* is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously. [Coroutines](https://kotlinlang.org/docs/coroutines-guide.html) were added to Kotlin in version 1.3 and are based on established concepts from other languages.

On Android, coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive.

Coroutines offers many benefits over other asynchronous solutions, including the following:
- **Lightweight**. You can run many coroutines on a single thread due to support for suspension, which doesn't block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations;
- **Fewer memory leaks**. Use structured concurrency to run operations within a scope;
- **Built-in cancellation support**. Cancellation is propagated automatically through the running coroutine hierarchy;
- **Jetpack integration**. Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency.

# Links
[AsyncTask](https://developer.android.com/reference/android/os/AsyncTask)

[WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager)

[RxJava For Android - RxAndroid](https://blog.mindorks.com/rxjava-for-android-rxandroid/)

[Kotlin coroutines on Android](https://developer.android.com/kotlin/coroutines)

# Next questions
[What's WorkManager?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20WorkManager.md)

[What are coroutines?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20are%20coroutines.md)

# Further Reading
[ReactiveX](https://reactivex.io/)

[Coroutines guide](https://kotlinlang.org/docs/coroutines-guide.html)
