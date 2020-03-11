# Launch modes

There are four different launch modes: 
- standard
- singleTop
- singleTask
- singleInstance

## Standard

Default. The system creates a new instance of the activity in the task from which it was started and routes the intent to it. The activity can be instantiated multiple times, each instance can belong to different tasks, and one task can have multiple instances.

## SingleTop

It acts almost the same as **standard** one which means that singleTop Activity instance could be created as many as we want. Only difference is if there already is an Activity instance with the same type at the top of stack in the caller Task, there would not be any new Activity created, instead an Intent will be sent to an existed Activity instance through `onNewIntent()` method.

In singleTop mode, you have to handle an incoming Intent in both `onCreate()` and `onNewIntent()` to make it works for all the cases.

## SingleTask

The system creates a new task and instantiates the activity at the root of the new task. However, if an instance of the activity already exists in a separate task, the system routes the intent to the existing instance through a call to its `onNewIntent()` method, rather than creating a new instance. Only one instance of the activity can exist at a time (a.k.a. Singleton).

## SingleInstance

Same as **singleTask**, except that the system doesn't launch any other activities into the task holding the instance. The activity is always the single and only member of its task; any activities started by this one open in a separate task.

## Links
https://developer.android.com/guide/components/activities/tasks-and-back-stack  
https://android.jlelse.eu/android-activity-launch-mode-e0df1aa72242   
https://inthecheesefactory.com/blog/understand-android-activity-launchmode/en
