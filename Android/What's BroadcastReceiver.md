# BroadcastReceiver
A broadcast receiver (receiver) is an Android component which allows you to register for system or application events.  Android apps can send or receive broadcast messages from the Android system and other Android apps, similar to the publish-subscribe design pattern. These broadcasts are sent when an event of interest occurs. For example, the Android system sends broadcasts when various system events occur, such as when the system boots up or the device starts charging. Apps can also send custom broadcasts, for example, to notify other apps of something that they might be interested in (for example, some new data has been downloaded).

Apps can register to receive specific broadcasts. When a broadcast is sent, the system automatically routes broadcasts to apps that have subscribed to receive that particular type of broadcast. Unlike activities, android `BroadcastReceiver` doesn’t contain any user interface. Broadcast receiver is generally implemented to delegate the tasks to services depending on the type of intent data that’s received.

## Receiving broadcasts
Apps can receive broadcasts in two ways:
- Manifest-declared receivers
- Context-registered receivers

### Manifest-declared receivers
If you declare a broadcast receiver in your manifest, the system launches your app (if the app is not already running) when the broadcast is sent.

**Note**: If your app targets API level 26 or higher, you cannot use the manifest to declare a receiver for *implicit* broadcasts (broadcasts that do not target your app specifically), except for a few implicit broadcasts that are [exempted from that restriction](https://developer.android.com/guide/components/broadcast-exceptions)

To declare a broadcast receiver in the manifest, perform the following steps:
1. Specify the `<receiver>` element in your app's manifest.

```
<receiver android:name=".MyBroadcastReceiver"  android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="android.intent.action.INPUT_METHOD_CHANGED" />
    </intent-filter>
</receiver>
```
The intent filters specify the broadcast actions your receiver subscribes to.

2. Subclass `BroadcastReceiver` and implement `onReceive(Context, Intent)`. The broadcast receiver in the following example logs and displays the contents of the broadcast:
```
private const val TAG = "MyBroadcastReceiver"

class MyBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        StringBuilder().apply {
            append("Action: ${intent.action}\n")
            append("URI: ${intent.toUri(Intent.URI_INTENT_SCHEME)}\n")
            toString().also { log ->
                Log.d(TAG, log)
                Toast.makeText(context, log, Toast.LENGTH_LONG).show()
            }
        }
    }
}
```
The system package manager registers the receiver when the app is installed. The receiver then becomes a separate entry point into your app which means that the system can start the app and deliver the broadcast if the app is not currently running.

The system creates a new `BroadcastReceiver` component object to handle each broadcast that it receives. This object is valid only for the duration of the call to `onReceive(Context, Intent)`. Once your code returns from this method, the system considers the component no longer active.

### Context-registered receivers
Context-registered receivers receive broadcasts as long as their registering context is valid. For an example, if you register within an `Activity` context, you receive broadcasts as long as the activity is not destroyed. If you register with the Application context, you receive broadcasts as long as the app is running.

To register a receiver with a context, perform the following steps:
1. Create an instance of `BroadcastReceiver`:
```
val br: BroadcastReceiver = MyBroadcastReceiver()
```

2. Create an IntentFilter and register the receiver by calling registerReceiver(BroadcastReceiver, IntentFilter):
```
val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION).apply {
    addAction(Intent.ACTION_AIRPLANE_MODE_CHANGED)
}
registerReceiver(br, filter)
```

To stop receiving broadcasts, call `unregisterReceiver(android.content.BroadcastReceiver)`. Be sure to unregister the receiver when you no longer need it or the context is no longer valid.

Be mindful of where you register and unregister the receiver, for example, if you register a receiver in `onCreate(Bundle)` using the activity's context, you should unregister it in `onDestroy()` to prevent leaking the receiver out of the activity context. If you register a receiver in `onResume()`, you should unregister it in `onPause()` to prevent registering it multiple times (If you don't want to receive broadcasts when paused, and this can cut down on unnecessary system overhead). Do not unregister in `onSaveInstanceState(Bundle)`, because this isn't called if the user moves back in the history stack.

## Effects on process state
The state of your `BroadcastReceiver` (whether it is running or not) affects the state of its containing process, which can in turn affect its likelihood of being killed by the system. For example, when a process executes a receiver (that is, currently running the code in its `onReceive()` method), it is considered to be a foreground process. The system keeps the process running except under cases of extreme memory pressure.

However, once your code returns from `onReceive()`, the `BroadcastReceiver` is no longer active. The receiver's host process becomes only as important as the other app components that are running in it. If that process hosts only a manifest-declared receiver (a common case for apps that the user has never or not recently interacted with), then upon returning from `onReceive()`, the system considers its process to be a low-priority process and may kill it to make resources available for other more important processes.

For this reason, you should not start long running background threads from a broadcast receiver. After `onReceive()`, the system can kill the process at any time to reclaim memory, and in doing so, it terminates the spawned thread running in the process. To avoid this, you should either call `goAsync()` (if you want a little more time to process the broadcast in a background thread) or schedule a `JobService` from the receiver using the `JobScheduler`, so the system knows that the process continues to perform active work.

## Security considerations and best practices
- If you don't need to send broadcasts to components outside of your app, then send and receive local broadcasts with the `LocalBroadcastManager` which is available in the Support Library. The `LocalBroadcastManager` is much more efficient (no interprocess communication needed) and allows you to avoid thinking about any security issues related to other apps being able to receive or send your broadcasts. Local Broadcasts can be used as a general purpose pub/sub event bus in your app without any overheads of system wide broadcasts.
- If many apps have registered to receive the same broadcast in their manifest, it can cause the system to launch a lot of apps, causing a substantial impact on both device performance and user experience. To avoid this, prefer using context registration over manifest declaration.
- Do not broadcast sensitive information using an implicit intent. The information can be read by any app that registers to receive the broadcast. 
- The namespace for broadcast actions is global. Make sure that action names and other strings are written in a namespace you own, or else you may inadvertently conflict with other apps.
- Because a receiver's `onReceive(Context, Intent)` method runs on the main thread, it should execute and return quickly. If you need to perform long running work, be careful about spawning threads or starting background services because the system can kill the entire process after `onReceive()` returns. 
- Do not start activities from broadcast receivers because the user experience is jarring; especially if there is more than one receiver. Instead, consider displaying a notification.

## Links
https://developer.android.com/guide/components/broadcasts  
https://www.vogella.com/tutorials/AndroidBroadcastReceiver/article.html  
https://www.journaldev.com/10356/android-broadcastreceiver-example-tutorial  
https://www.tutorialspoint.com/android/android_broadcast_receivers.htm
