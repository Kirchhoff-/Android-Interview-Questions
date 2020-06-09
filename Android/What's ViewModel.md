# ViewModel
The `ViewModel` class is designed to store and manage UI-related data in a lifecycle conscious way. The `ViewModel` class allows data to survive configuration changes such as screen rotations.

The Android framework manages the lifecycles of UI controllers, such as activities and fragments. The framework may decide to destroy or re-create a UI controller in response to certain user actions or device events that are completely out of your control.

If the system destroys or re-creates a UI controller, any transient UI-related data you store in them is lost. For example, your app may include a list of users in one of its activities. When the activity is re-created for a configuration change, the new activity has to re-fetch the list of users.  For simple data, the activity can use the `onSaveInstanceState()` method and restore its data from the bundle in `onCreate()`, but this approach is only suitable for small amounts of data that can be serialized then deserialized, not for potentially large amounts of data like a list of users or bitmaps.

Another problem is that UI controllers frequently need to make asynchronous calls that may take some time to return. The UI controller needs to manage these calls and ensure the system cleans them up after it's destroyed to avoid potential memory leaks. This management requires a lot of maintenance, and in the case where the object is re-created for a configuration change, it's a waste of resources since the object may have to reissue calls it has already made.

UI controllers such as activities and fragments are primarily intended to display UI data, react to user actions, or handle operating system communication, such as permission requests. Requiring UI controllers to also be responsible for loading data from a database or network adds bloat to the class. Assigning excessive responsibility to UI controllers can result in a single class that tries to handle all of an app's work by itself, instead of delegating work to other classes. Assigning excessive responsibility to the UI controllers in this way also makes testing a lot harder.

## Implement a ViewModel
Architecture Components provides `ViewModel` helper class for the UI controller that is responsible for preparing data for the UI. `ViewModel` objects are automatically retained during configuration changes so that data they hold is immediately available to the next activity or fragment instance. 

Example: 
```
class MyViewModel : ViewModel() {
    private val users: MutableLiveData<List<User>> by lazy {
        MutableLiveData().also {
            loadUsers()
        }
    }

    fun getUsers(): LiveData<List<User>> {
        return users
    }

    private fun loadUsers() {
        // Do an asynchronous operation to fetch users.
    }
}
```
You can then access the list from an activity as follows:
```
class MyActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        // Create a ViewModel the first time the system calls an activity's onCreate() method.
        // Re-created activities receive the same MyViewModel instance created by the first activity.

        // Use the 'by viewModels()' Kotlin property delegate
        // from the activity-ktx artifact
        val model: MyViewModel by viewModels()
        model.getUsers().observe(this, Observer<List<User>>{ users ->
            // update UI
        })
    }
}
```
If the activity is re-created, it receives the same `MyViewModel` instance that was created by the first activity. When the owner activity is finished, the framework calls the `ViewModel` objects's `onCleared()` method so that it can clean up resources.

## The lifecycle of a ViewModel
`ViewModel` objects are scoped to the `Lifecycle` passed to the `ViewModelProvider` when getting the `ViewModel`. The `ViewModel` remains in memory until the `Lifecycle` it's scoped to goes away permanently: in the case of an activity, when it finishes, while in the case of a fragment, when it's detached.

![](./res/viewmodel_lifecycle.png "ViewModel lifecycle")

You usually request a `ViewModel` the first time the system calls an activity object's `onCreate()` method. The system may call `onCreate()` several times throughout the life of an activity, such as when a device screen is rotated. The `ViewModel` exists from when you first request a `ViewModel` until the activity is finished and destroyed.

## Links
https://developer.android.com/topic/libraries/architecture/viewmodel  
https://developer.android.com/reference/androidx/lifecycle/ViewModel  
https://medium.com/androiddevelopers/viewmodels-a-simple-example-ed5ac416317e  
https://android.jlelse.eu/dive-deep-into-androids-viewmodel-android-architecture-components-e0a7ded26f70  
