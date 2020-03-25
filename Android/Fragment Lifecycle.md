# Fragment Lifecycle

![](./res/fragment_lifecycle.png "Fragment lifecycle")

## Lifecycle Methods

- `onAttach()` - Called when the fragment has been associated with the activity (the Activity is passed in here). 

- `onCreate()` - The system calls this when creating the fragment. Within your implementation, you should initialize essential components of the fragment that you want to retain when the fragment is paused or stopped, then resumed.

- `onCreateView()` - The system calls this when it's time for the fragment to draw its user interface for the first time. To draw a UI for your fragment, you must return a `View` from this method that is the root of your fragment's layout. You can return null if the fragment does not provide a UI.

- `onActivityCreated()` - This will be called to indicate that the activity’s `onCreate()` has completed. If there is something that’s needed to be initialised in the fragment that depends upon the activity’s` onCreate()` having completed its work then `onActivityCreated()` can be used for that initialisation work.

- `onStart()` - Called once the fragment gets visible.

- `onResume()` - Called when the fragment will start interacting with the user.

- `onPause()` - The system calls this method as the first indication that the user is leaving the fragment (though it doesn't always mean the fragment is being destroyed). This is usually where you should commit any changes that should be persisted beyond the current user session (because the user might not come back).

- `onStop()` - Called when you are no longer visible to the user. 

- `onDestroyView()` - Called when the view hierarchy associated with the fragment is being removed.

- `onDestroy()` - Called to do final clean up of the fragment’s state but not guaranteed to be called by the Android platform.

- `onDetach()` - Called when the fragment is being disassociated from the activity.

## Fragment States

Managing the lifecycle of a fragment is a lot like managing the lifecycle of an activity. Like an activity, a fragment can exist in three states:

- **Resumed** - The fragment is visible in the running activity.

- **Paused** - Another activity is in the foreground and has focus, but the activity in which this fragment lives is still visible (the foreground activity is partially transparent or doesn't cover the entire screen).

- **Stopped** - The fragment isn't visible. Either the host activity has been stopped or the fragment has been removed from the activity but added to the back stack. A stopped fragment is still alive (all state and member information is retained by the system). However, it is no longer visible to the user and is killed if the activity is killed.


## Links
https://developer.android.com/guide/components/fragments  
https://www.journaldev.com/9266/android-fragment-lifecycle  
https://www.techyourchance.com/android-fragment-lifecycle/  
