# `ViewModel`
The `ViewModel` class is a business logic or screen level state holder. It exposes state to the UI and encapsulates related business logic. Its principal advantage is that it caches state and persists it through configuration changes. This means that your UI doesn’t have to fetch data again when navigating between activities, or following configuration changes, such as when rotating the screen.<sup>[1](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=en#:~:text=The%20ViewModel%20class,rotating%20the%20screen.)</sup>

A `ViewModel` is always created in association with a scope (a fragment or an activity) and will be retained as long as the scope is alive. E.g. if it is an `Activity`, until it is finished.<sup>[2](https://developer.android.com/reference/androidx/lifecycle/ViewModel#:~:text=A%20ViewModel%20is%20always%20created%20in%20association%20with%20a%20scope%20(a%20fragment%20or%20an%20activity)%20and%20will%20be%20retained%20as%20long%20as%20the%20scope%20is%20alive.%20E.g.%20if%20it%20is%20an%20Activity%2C%20until%20it%20is%20finished.)</sup>

The purpose of the `ViewModel` is to acquire and keep the information that is necessary for an `Activity` or a `Fragment`. The `Activity` or the `Fragment` should be able to observe changes in the `ViewModel`. ViewModels usually expose this information via `LiveData`<sup>[3](https://developer.android.com/reference/androidx/lifecycle/ViewModel#:~:text=The%20purpose%20of%20the%20ViewModel%20is%20to%20acquire%20and%20keep%20the%20information%20that%20is%20necessary%20for%20an%20Activity%20or%20a%20Fragment.%20The%20Activity%20or%20the%20Fragment%20should%20be%20able%20to%20observe%20changes%20in%20the%20ViewModel.%20ViewModels%20usually%20expose%20this%20information%20via%20androidx.lifecycle.LiveData)</sup> (or via `Flow`).

ViewModel's only responsibility is to manage the data for the UI. It **should never** access your view hierarchy or hold a reference back to the `Activity` or the `Fragment`.

Typical usage from an `Activity` standpoint would be:
```
class UserActivity : ComponentActivity {
    private val viewModel by viewModels<UserViewModel>()

    override fun onCreate(savedInstanceState: Bundle) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.user_activity_layout)
        viewModel.user.observe(this) { user: User ->
            // update ui.
        }
        requireViewById(R.id.button).setOnClickListener {
            viewModel.doAction()
        }
    }
}
```

`ViewModel` would be:
```
class UserViewModel : ViewModel {
    private val userLiveData = MutableLiveData<User>()
    val user: LiveData<User> get() = userLiveData

    init {
        // trigger user load.
    }

    fun doAction() {
        // depending on the action, do necessary business logic calls and update the
        // userLiveData.
    }
}
```

ViewModels can also be used as a communication layer between different Fragments of an `Activity`. Each `Fragment` can acquire the `ViewModel` using the same key via their `Activity`. This allows communication between Fragments in a de-coupled fashion such that they never need to talk to the other `Fragment` directly<sup>[4](https://developer.android.com/reference/androidx/lifecycle/ViewModel#:~:text=ViewModel%27s%20only%20responsibility,UserViewModel%3E()%0A%7D)</sup>:
```
class MyFragment : Fragment {
  val viewModel by activityViewModels<UserViewModel>()
}
```

`ViewModel` includes support for Kotlin coroutines. It is able to persist asynchronous work in the same manner as it persists UI state.<sup>[5](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=en#:~:text=ViewModel%20includes%20support%20for%20Kotlin%20coroutines.%20It%20is%20able%20to%20persist%20asynchronous%20work%20in%20the%20same%20manner%20as%20it%20persists%20UI%20state.)</sup>

## [`ViewModel` benefits](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=en#viewmodel-benefits)
The alternative to a `ViewModel` is a plain class that holds the data you display in your UI. This can become a problem when navigating between activities or Navigation destinations. Doing so destroys that data if you don't store it using the saving instance state mechanism. `ViewModel` provides a convenient API for data persistence that resolves this issue.

The key benefits of the `ViewModel` class are essentially two:
- It allows you to persist UI state;
- It provides access to business logic.

## [The lifecycle of a `ViewModel`](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=en#lifecycle)
The lifecycle of a `ViewModel` is tied directly to its scope. A `ViewModel` remains in memory until the `ViewModelStoreOwner` to which it is scoped disappears. This may occur in the following contexts:
- In the case of an activity, when it finishes;
- In the case of a fragment, when it detaches;
- In the case of a Navigation entry, when it's removed from the back stack.

This makes ViewModels a great solution for storing data that survives configuration changes.

Figure below illustrates the various lifecycle states of an activity as it undergoes a rotation and then is finished. The illustration also shows the lifetime of the `ViewModel` next to the associated activity lifecycle. This particular diagram illustrates the states of an activity. The same basic states apply to the lifecycle of a fragment.

![](./res/viewmodel_lifecycle.png "ViewModel lifecycle")

You usually request a `ViewModel` the first time the system calls an activity object's `onCreate()` method. The system may call `onCreate()` several times throughout the existence of an activity, such as when a device screen is rotated. The `ViewModel` exists from when you first request a `ViewModel` until the activity is finished and destroyed.

# Links
[ViewModel overview](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=en)

[ViewModel](https://developer.android.com/reference/androidx/lifecycle/ViewModel)

# Further reading
[ViewModels : A Simple Example](https://medium.com/androiddevelopers/viewmodels-a-simple-example-ed5ac416317e)

[How ViewModels Work on Android](https://betterprogramming.pub/everything-to-understand-about-viewmodel-400e8e637a58)

[Using ViewModels to Retain State on Android](https://www.nutrient.io/blog/using-viewmodels-to-retain-state-on-android/)

[View Model Creation in Android — Android Architecture Components & Kotlin](https://proandroiddev.com/view-model-creation-in-android-android-architecture-components-kotlin-ce9f6b93a46b)

[Dive deep into Android’s ViewModel — Android Architecture Components](https://medium.com/android-news/dive-deep-into-androids-viewmodel-android-architecture-components-e0a7ded26f70)

# Next questions
[What's `LiveData`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20LiveData.md)

[What do you know about `StateFlow` and `SharedFlow`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20StateFlow%20and%20SharedFlow.md)

[How to communicate with fragments?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/How%20to%20communicate%20with%20fragments.md)
