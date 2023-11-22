# taskAffinity
The `taskAffinity` is the attribute that is declared in the AndroidManifest file.

An *affinity* indicates which task an activity "prefers" to belong to. By default, all the activities from the same app have an affinity for each other: they "prefer" to be in the same task.

However, you can modify the default affinity for an activity. Activities defined in different apps can share an affinity, and activities defined in the same app can be assigned different task affinities.

You can modify an activity's affinity using the `taskAffinity` attribute of the `<activity>` element.

The `taskAffinity` attribute takes a string value that must be different than the default package name declared in the `<manifest>` element, because the system uses that name to identify the default task affinity for the app.

The affinity comes into play in two circumstances:

**When the intent that launches an activity contains the `FLAG_ACTIVITY_NEW_TASK` flag**.
A new activity, by default, is launched into the task of the activity that called `startActivity()`. It's pushed onto the same back stack as the caller.

  However, if the intent passed to `startActivity()` contains the `FLAG_ACTIVITY_NEW_TASK` flag, the system looks for a different task to house the new activity. Often, this is a new task. However, it doesn't have to be. If there's an existing task with the same affinity as the new activity, the activity is launched into that task. If not, it begins a new task.

If this flag causes an activity to begin a new task and the user uses the Home button or gesture to leave it, there must be some way for the user to navigate back to the task. Some entities, such as the notification manager, always start activities in an external task, never as part of their own, so they always put `FLAG_ACTIVITY_NEW_TASK` in the intents they pass to `startActivity()`.

If an external entity that might use this flag can invoke your activity, take care that the user has an independent way to get back to the task that's started, such as with a launcher icon, where the root activity of the task has a `CATEGORY_LAUNCHER` intent filter. For more information, see the section about starting tasks.

**When an activity has its `allowTaskReparenting` attribute set to "true".**
In this case, the activity can move from the task it starts in to the task it has an affinity for when that task comes to the foreground.

For example, suppose an activity that reports weather conditions in selected cities is defined as part of a travel app. It has the same affinity as other activities in the same app, the default app affinity, and it can be re-parented with this attribute.

When one of your activities starts the weather reporter activity, it initially belongs to the same task as your activity. However, when the travel app's task comes to the foreground, the weather reporter activity is reassigned to that task and displayed within it.

**Note**: If an APK file contains more than one "app" from the user's point of view, you probably want to use the taskAffinity attribute to assign different affinities to the activities associated with each "app".

# Links
[Handle affinities](https://developer.android.com/guide/components/activities/tasks-and-back-stack#Affinities)

# Further reading
[Playing with Android Task Affinity and Launch Modes](https://medium.com/@veeresh.charantimath8/playing-with-android-task-affinity-and-launch-modes-5c36a0421e83)

[Android Tasks: Once and for all](https://medium.com/android-news/https-medium-com-yashsoniweb-android-tasks-ffbd547ff5b8)
