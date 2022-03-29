# Activity Result APIs
While the underlying `startActivityForResult()` and `onActivityResult()` APIs are available on the `Activity` class on all API levels, it is strongly recommended to use the Activity Result APIs introduced in AndroidX `Activity` and `Fragment`.

## [Registering a callback for an Activity Result](https://developer.android.com/training/basics/intents/result#register)
When starting an activity for a result, it is possible (and, in cases of memory-intensive operations such as camera usage, almost certain) that your process and your activity will be destroyed due to low memory.

For this reason, the Activity Result APIs decouple the result callback from the place in your code where you launch the other activity. As the result callback needs to be available when your process and activity are recreated, the callback must be unconditionally registered every time your activity is created, even if the logic of launching the other activity only happens based on user input or other business logic.

When in a `ComponentActivity` or a `Fragment`, the Activity Result APIs provide a `registerForActivityResult()` API for registering the result callback. `registerForActivityResult()` takes an `ActivityResultContract` and an `ActivityResultCallback` and returns an `ActivityResultLauncher` which you’ll use to launch the other activity.

An `ActivityResultContract` defines the input type needed to produce a result along with the output type of the result. The APIs provide default contracts for basic intent actions like taking a picture, requesting permissions, and so on. You can also create your own custom contracts.

`ActivityResultCallback` is a single method interface with an `onActivityResult()` method that takes an object of the output type defined in the `ActivityResultContract`:

```
val getContent = registerForActivityResult(GetContent()) { uri: Uri? ->
    // Handle the returned Uri
}
```

If you have multiple activity result calls that either use different contracts or want separate callbacks, you can call `registerForActivityResult()` multiple times to register multiple `ActivityResultLauncher` instances. You must always call `registerForActivityResult()` in the same order for each creation of your fragment or activity to ensure that the inflight results are delivered to the correct callback.

`registerForActivityResult()` is safe to call before your fragment or activity is created, allowing it to be used directly when declaring member variables for the returned `ActivityResultLauncher` instances.

## [Launching an activity for result](https://developer.android.com/training/basics/intents/result#launch)
While `registerForActivityResult()` registers your callback, it does **not** launch the other activity and kick off the request for a result. Instead, this is the responsibility of the returned `ActivityResultLauncher` instance.

If input exists, the launcher takes the input that matches the type of the `ActivityResultContract`. Calling `launch()` starts the process of producing the result. When the user is done with the subsequent activity and returns, the `onActivityResult()` from the `ActivityResultCallback` is then executed, as shown in the following example:

```
val getContent = registerForActivityResult(GetContent()) { uri: Uri? ->
    // Handle the returned Uri
}

override fun onCreate(savedInstanceState: Bundle?) {
    // ...

    val selectButton = findViewById<Button>(R.id.select_button)

    selectButton.setOnClickListener {
        // Pass in the mime type you'd like to allow the user to select
        // as the input
        getContent.launch("image/*")
    }
}
```

An overloaded version of `launch()` allows you to pass an `ActivityOptionsCompat` in addition to the input.

## [Receiving an activity result in a separate class](https://developer.android.com/training/basics/intents/result#separate)
While the `ComponentActivity` and `Fragment` classes implement the `ActivityResultCaller` interface to allow you to use the `registerForActivityResult()` APIs, you can also receive the activity result in a separate class that does not implement `ActivityResultCaller` by using `ActivityResultRegistry` directly.

For example, you might want to implement a `LifecycleObserver` that handles registering a contract along with launching the launcher:
```
class MyLifecycleObserver(private val registry : ActivityResultRegistry)
        : DefaultLifecycleObserver {
    lateinit var getContent : ActivityResultLauncher<String>

    override fun onCreate(owner: LifecycleOwner) {
        getContent = registry.register("key", owner, GetContent()) { uri ->
            // Handle the returned Uri
        }
    }

    fun selectImage() {
        getContent.launch("image/*")
    }
}

class MyFragment : Fragment() {
    lateinit var observer : MyLifecycleObserver

    override fun onCreate(savedInstanceState: Bundle?) {
        // ...

        observer = MyLifecycleObserver(requireActivity().activityResultRegistry)
        lifecycle.addObserver(observer)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        val selectButton = view.findViewById<Button>(R.id.select_button)

        selectButton.setOnClickListener {
            // Open the activity to select an image
            observer.selectImage()
        }
    }
}
```

When using the `ActivityResultRegistry` APIs, it's strongly recommended to use the APIs that take a `LifecycleOwner`, as the `LifecycleOwner` automatically removes your registered launcher when the `Lifecycle` is destroyed. However, in cases where a `LifecycleOwner` is not available, each `ActivityResultLauncher` class allows you to manually call `unregister()` as an alternative.

# Links
[Getting a result from an activity](https://developer.android.com/training/basics/intents/result)

# Futher reading
[Activity Results API: A better way to pass data between Activities](https://proandroiddev.com/is-onactivityresult-deprecated-in-activity-results-api-lets-deep-dive-into-it-302d5cf6edd)

[The right way to get a result (Part I): Activity Result API](https://medium.com/e-legion/the-right-way-to-get-a-result-part-i-activity-result-api-6efbcaa5600d)
