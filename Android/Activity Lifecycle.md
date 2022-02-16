# Activity Lifecycle

As a user navigates through, out of, and back to your app, the `Activity` instances in your app transition through different states in their lifecycle. The `Activity` class provides a number of callbacks that allow the activity to know that a state has changed: that the system is creating, stopping, or resuming an activity, or destroying the process in which the activity resides.

To navigate transitions between stages of the activity lifecycle, the `Activity` class provides a core set of six callbacks: `onCreate()`, `onStart()`, `onResume()`, `onPause()`, `onStop()`, and `onDestroy()`. The system invokes each of these callbacks as an activity enters a new state.

![](./res/activity_lifecycle.png "Activity lifecycle")

As the user begins to leave the activity, the system calls methods to dismantle the activity. In some cases, this dismantlement is only partial; the activity still resides in memory (such as when the user switches to another app), and can still come back to the foreground. If the user returns to that activity, the activity resumes from where the user left off.

## [onCreate()](https://developer.android.com/guide/components/activities/activity-lifecycle#oncreate)

You must implement this callback, which fires when the system first creates the activity. On activity creation, the activity enters the *Created* state. In the `onCreate()` method, you perform basic application startup logic that should happen only once for the entire life of the activity. For example, your implementation of `onCreate()` might bind data to lists, associate the activity with a `ViewModel`, and instantiate some class-scope variables. This method receives the parameter `savedInstanceState`, which is a `Bundle` object containing the activity's previously saved state. If the activity has never existed before, the value of the `Bundle` object is `null`.

If you have a lifecycle-aware component that is hooked up to the lifecycle of your activity it will receive the `ON_CREATE` event. The method annotated with `@OnLifecycleEvent` will be called so your lifecycle-aware component can perform any setup code it needs for the created state.

Your activity does not reside in the Created state. After the `onCreate()` method finishes execution, the activity enters the *Started* state, and the system calls the `onStart()` and `onResume()` methods in quick succession. 

## [onStart()](https://developer.android.com/guide/components/activities/activity-lifecycle#onstart)

When the activity enters the Started state, the system invokes this callback. The `onStart()` call makes the activity visible to the user, as the app prepares for the activity to enter the foreground and become interactive. For example, this method is where the app initializes the code that maintains the UI.

When the activity moves to the started state, any lifecycle-aware component tied to the activity's lifecycle will receive the `ON_START` event.

The `onStart()` method completes very quickly and, as with the Created state, the activity does not stay resident in the Started state. Once this callback finishes, the activity enters the Resumed state, and the system invokes the `onResume()` method.

## [onResume()](https://developer.android.com/guide/components/activities/activity-lifecycle#onresume)

When the activity enters the Resumed state, it comes to the foreground, and then the system invokes the `onResume()` callback. This is the state in which the app interacts with the user. The app stays in this state until something happens to take focus away from the app. Such an event might be, for instance, receiving a phone call, the user’s navigating to another activity, or the device screen’s turning off.

When the activity moves to the resumed state, any lifecycle-aware component tied to the activity's lifecycle will receive the `ON_RESUME` event. This is where the lifecycle components can enable any functionality that needs to run while the component is visible and in the foreground, such as starting a camera preview.

When an interruptive event occurs, the activity enters the *Paused* state, and the system invokes the `onPause()` callback.

If the activity returns to the Resumed state from the Paused state, the system once again calls `onResume()` method. For this reason, you should implement `onResume()` to initialize components that you release during `onPause()`, and perform any other initializations that must occur each time the activity enters the Resumed state.

## [onPause()](https://developer.android.com/guide/components/activities/activity-lifecycle#onpause)

The system calls this method as the first indication that the user is leaving your activity (though it does not always mean the activity is being destroyed); it indicates that the activity is no longer in the foreground (though it may still be visible if the user is in multi-window mode). Use the `onPause()` method to pause or adjust operations that should not continue (or should continue in moderation) while the `Activity` is in the Paused state, and that you expect to resume shortly. There are several reasons why an activity may enter this state.

When the activity moves to the paused state, any lifecycle-aware component tied to the activity's lifecycle will receive the `ON_PAUSE` event. This is where the lifecycle components can stop any functionality that does not need to run while the component is not in the foreground, such as stopping a camera preview.

`onPause()` execution is very brief, and does not necessarily afford enough time to perform save operations. For this reason, you should not use `onPause()` to save application or user data, make network calls, or execute database transactions; such work may not complete before the method completes. Instead, you should perform heavy-load shutdown operations during `onStop()`.

## [onStop()](https://developer.android.com/guide/components/activities/activity-lifecycle#onstop)

When your activity is no longer visible to the user, it has entered the *Stopped* state, and the system invokes the `onStop()` callback. This may occur, for example, when a newly launched activity covers the entire screen. The system may also call `onStop()` when the activity has finished running, and is about to be terminated.

When the activity moves to the stopped state, any lifecycle-aware component tied to the activity's lifecycle will receive the `ON_STOP` event. This is where the lifecycle components can stop any functionality that does not need to run while the component is not visible on the screen.

In the `onStop()` method, the app should release or adjust resources that are not needed while the app is not visible to the user. For example, your app might pause animations or switch from fine-grained to coarse-grained location updates. Using `onStop()` instead of `onPause()` ensures that UI-related work continues, even when the user is viewing your activity in multi-window mode.

You should also use `onStop()` to perform relatively CPU-intensive shutdown operations. For example, if you can't find a more opportune time to save information to a database, you might do so during `onStop()`.

## [onDestroy()](https://developer.android.com/guide/components/activities/activity-lifecycle#ondestroy)
`onDestroy()` is called before the activity is destroyed. The system invokes this callback either because:
- the activity is finishing (due to the user completely dismissing the activity or due to `finish()` being called on the activity), or
- the system is temporarily destroying the activity due to a configuration change (such as device rotation or multi-window mode)

When the activity moves to the destroyed state, any lifecycle-aware component tied to the activity's lifecycle will receive the `ON_DESTROY` event. This is where the lifecycle components can clean up anything it needs to before the Activity is destroyed.

Instead of putting logic in your Activity to determine why it is being destroyed you should use a `ViewModel` object to contain the relevant view data for your Activity. If the Activity is going to be recreated due to a configuration change the ViewModel does not have to do anything since it will be preserved and given to the next Activity instance. If the Activity is not going to be recreated then the ViewModel will have the `onCleared()` method called where it can clean up any data it needs to before being destroyed.

You can distinguish between these two scenarios with the `isFinishing()` method.

If the activity is finishing, `onDestroy()` is the final lifecycle callback the activity receives. If `onDestroy()` is called as the result of a configuration change, the system immediately creates a new activity instance and then calls `onCreate()` on that new instance in the new configuration.

The `onDestroy()` callback should release all resources that have not yet been released by earlier callbacks such as `onStop()`.

# Links
[The Activity Lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle)

# Further reading
[Android activity life cycle - what are all these methods for?](https://stackoverflow.com/questions/8515936/android-activity-life-cycle-what-are-all-these-methods-for) 
