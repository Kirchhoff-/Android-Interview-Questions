# Service vs IntentService
A `Service` is an application component that can perform long-running operations in the background, and it doesn't provide a user interface. Another application component can start a service, and it continues to run in the background even if the user switches to another application. Additionally, a component can bind to a service to interact with it and even perform interprocess communication (IPC).

The `Service` class is the base class for all services. When you extend this class, it's important to create a new thread in which the service can complete all of its work; the service uses your application's main thread by default, which can slow the performance of any activity that your application is running.

The Android framework also provides the `IntentService` subclass of `Service` that uses a worker thread to handle all of the start requests, one at a time. `IntentService` is very similura to queue where it handles request (intents) from clients. Client can send the request to `IntentService` by using command - `Context.startService(Intent)`. For implements `IntentService` you should extend IntentService class and implements method - `onHandleIntent(Intent)`

Using `IntentService` is not recommended for new apps as it will not work well starting with Android 8 Oreo, due to the introduction of Background execution limits. Moreover, it's deprecated starting with Android 11. You can use `JobIntentService` as a replacement for `IntentService` that is compatible with newer versions of Android.

| `Service`  | `IntentService`  |
|---|---|
| Is invoked using `startService()`  | Is invoked using `Intent` |
| Can be invoked from any thread  | Can in invoked from the Main thread only  |
| Runs background operations on the Main Thread of the Application by default. Hence it can block your Application’s UI  | Creates a separate worker thread to run background operations  |
| Invoked multiple times would create multiple instances.  |  Invoked multiple times won’t create multiple instances |
| Needs to be stopped using `stopSelf()` or `stopService()`  | Automatically stops after the queue is completed. No need to trigger `stopService()` or `stopSelf()`  |
| Can run parallel operations  | Multiple intent calls are automatically queued and they would be executed sequentially |

## Links
https://developer.android.com/guide/components/services  
https://www.journaldev.com/20735/android-intentservice-broadcastreceiver  
https://stackoverflow.com/questions/15524280/service-vs-intentservice-in-the-android-platform  
