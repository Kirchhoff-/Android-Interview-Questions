# Tasks and the back stack
A *task* is a collection of activities that users interact with when trying to do something in your app. These activities are arranged in a stack called the *back stack* in the order in which each activity is opened.

For example, an email app might have one activity to show a list of new messages. When the user selects a message, a new activity opens to view that message. This new activity is added to the back stack. Then, when the user taps or gestures Back, that new activity finishes and is popped off the stack.

## [Lifecycle of a task and its back stack](https://developer.android.com/guide/components/activities/tasks-and-back-stack#life-cycle)
The device Home screen is the starting place for most tasks. When a user touches the icon for an app or shortcut in the app launcher or on the Home screen, that app's task comes to the foreground. If no task exists for the app, then a new task is created and the main activity for that app opens as the root activity in the stack.

When the current activity starts another, the new activity is pushed on the top of the stack and takes focus. The previous activity remains in the stack, but is stopped. When an activity is stopped, the system retains the current state of its user interface. When the user performs the back action, the current activity is popped from the top of the stack and destroyed. The previous activity resumes, and the previous state of its UI is restored.

Activities in the stack are never rearranged, only pushed onto and popped from the stack as they are started by the current activity and dismissed by the user through the Back button or gesture. Therefore, the back stack operates as a *last in, first out* object structure. Figure below shows a timeline with activities being pushed onto and popped from a back stack.

![](./res/diagram_backstack.png "Backstack")

As the user continues to tap or gesture Back, each activity in the stack is popped off to reveal the previous one, until the user returns to the Home screen or to whichever activity was running when the task began. When all activities are removed from the stack, the task no longer exists. 

## [Background and foreground tasks](https://developer.android.com/guide/components/activities/tasks-and-back-stack#background-foreground)
A task is a cohesive unit that can move to the *background* when a user begins a new task or goes to the Home screen. While in the background, all the activities in the task are stopped, but the back stack for the task remains intact—the task loses focus while another task takes place, as shown in image below. A task can then return to the *foreground* so users can pick up where they left off.

![](./res/diagram_multitasking.png "Multitasking")

**Note**: Multiple tasks can be held in the background at once. However, if the user runs many background tasks at the same time, the system might begin destroying background activities to recover memory. If this happens, the activity states are lost.

## [Back tap behavior for root launcher activities](https://developer.android.com/guide/components/activities/tasks-and-back-stack#back-press-behavior)
Root launcher activities are activities that declare an intent filter with both `ACTION_MAIN` and `CATEGORY_LAUNCHER`.  These activities are unique because they act as entry points into your app from the app launcher and are used to start a task.

When a user taps or gestures Back from a root launcher activity, the system handles the event differently depending on the version of Android that the device is running:
- **System behavior on Android 11 and lower**: The system finishes the activity.
- **System behavior on Android 12 and higher**: The system moves the activity and its task to the background instead of finishing the activity. This behavior matches the default system behavior when navigating out of an app using the Home button or gesture. In most cases, this behavior means that users can more quickly resume your app from a warm state, instead of having to completely restart the app from a cold state.

## [Manage tasks](https://developer.android.com/guide/components/activities/tasks-and-back-stack#ManagingTasks)
Android manages tasks and the back stack by placing all activities started in succession in the same task, in a last in, first out stack. This works great for most apps, and you usually don't have to worry about how your activities are associated with tasks or how they exist in the back stack.

However, you might decide that you want to interrupt the normal behavior. For example, you might want an activity in your app to begin a new task when it is started, instead of being placed within the current task. Or, when you start an activity, you might want to bring forward an existing instance of it, instead of creating a new instance on top of the back stack. Or you might want your back stack to be cleared of all activities except for the root activity when the user leaves the task.

You can do these things and more using attributes in the `<activity>` manifest element and flags in the intent that you pass to `startActivity()`.

These are the principal `<activity>` attributes that you can use to manage tasks:
- `taskAffinity`
- `launchMode`
- `allowTaskReparenting`
- `clearTaskOnLaunch`
- `alwaysRetainTaskState`
- `finishOnTaskLaunch`

# Links
[Tasks and the back stack](https://developer.android.com/guide/components/activities/tasks-and-back-stack)

# Next questions
[What launch modes do you know?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20launch%20modes%20do%20you%20know%3F.md)
