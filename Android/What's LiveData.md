# LiveData
`LiveData` is an observable data holder class. Unlike a regular observable, `LiveData` is lifecycle-aware, meaning it respects the lifecycle of other app components, such as activities, fragments, or services. This awareness ensures `LiveData` only updates app component observers that are in an active lifecycle state.

`LiveData` considers an observer, which is represented by the `Observer` class, to be in an active state if its lifecycle is in the `STARTED` or `RESUMED` state. `LiveData` only notifies active observers about updates. Inactive observers registered to watch `LiveData` objects aren't notified about changes.

You can register an observer paired with an object that implements the `LifecycleOwner` interface. This relationship allows the observer to be removed when the state of the corresponding `Lifecycle` object changes to `DESTROYED`. This is especially useful for activities and fragments because they can safely observe `LiveData` objects and not worry about leaks—activities and fragments are instantly unsubscribed when their lifecycles are destroyed.

## Work with LiveData objects

Follow these steps to work with `LiveData` objects:
- Create an instance of `LiveData` to hold a certain type of data. This is usually done within your `ViewModel` class.
- Create an `Observer` object that defines the `onChanged()` method, which controls what happens when the `LiveData` object's held data changes. You usually create an `Observer` object in a UI controller, such as an activity or fragment.
- Attach the `Observer` object to the `LiveData` object using the `observe()` method. The `observe()` method takes a `LifecycleOwner` object. This subscribes the `Observer` object to the `LiveData` object so that it is notified of changes. You usually attach the `Observer` object in a UI controller, such as an activity or fragment.

When you update the value stored in the `LiveData` object, it triggers all registered observers as long as the attached `LifecycleOwner` is in the active state.

`LiveData` allows UI controller observers to subscribe to updates. When the data held by the `LiveData` object changes, the UI automatically updates in response.

### Create LiveData objects
`LiveData` is a wrapper that can be used with any data, including objects that implement `Collections`, such as `List`. A `LiveData` object is usually stored within a `ViewModel ` object and is accessed via a getter method, as demonstrated in the following example:
```
class NameViewModel : ViewModel() {

    // Create a LiveData with a String
    val currentName: MutableLiveData<String> by lazy {
        MutableLiveData<String>()
    }

    // Rest of the ViewModel...
}
```

Initially, the data in a `LiveData` object is not set.

### Observe LiveData objects
In most cases, an app component’s `onCreate()` method is the right place to begin observing a `LiveData` object for the following reasons:
- To ensure the system doesn’t make redundant calls from an activity or fragment’s `onResume()` method.
- To ensure that the activity or fragment has data that it can display as soon as it becomes active. As soon as an app component is in the `STARTED` state, it receives the most recent value from the `LiveData` objects it’s observing. This only occurs if the `LiveData` object to be observed has been set.

Generally, `LiveData` delivers updates only when data changes, and only to active observers. An exception to this behavior is that observers also receive an update when they change from an inactive to an active state. Furthermore, if the observer changes from inactive to active a second time, it only receives an update if the value has changed since the last time it became active.

The following sample code illustrates how to start observing a `LiveData` object:
```
class NameActivity : AppCompatActivity() {

    // Use the 'by viewModels()' Kotlin property delegate
    // from the activity-ktx artifact
    private val model: NameViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Other code to setup the activity...

        // Create the observer which updates the UI.
        val nameObserver = Observer<String> { newName ->
            // Update the UI, in this case, a TextView.
            nameTextView.text = newName
        }

        // Observe the LiveData, passing in this activity as the LifecycleOwner and the observer.
        model.currentName.observe(this, nameObserver)
    }
}
```

After `observe()` is called with `nameObserver` passed as parameter, `onChanged()` is immediately invoked providing the most recent value stored in `mCurrentName`. If the `LiveData` object hasn't set a value in `mCurrentName`, `onChanged()` is not called.

### Update LiveData objects
`LiveData` has no publicly available methods to update the stored data. The `MutableLiveData` class exposes the `setValue(T)` and `postValue(T)` methods publicly and you must use these if you need to edit the value stored in a `LiveData` object. Usually `MutableLiveData` is used in the `ViewModel` and then the `ViewModel` only exposes immutable `LiveData` objects to the observers.

After you have set up the observer relationship, you can then update the value of the `LiveData` object, as illustrated by the following example, which triggers all observers when the user taps a button:
```
button.setOnClickListener {
    val anotherName = "John Doe"
    model.currentName.setValue(anotherName)
}
```

Calling `setValue(T)` in the example results in the observers calling their `onChanged()` methods with the value John Doe. The example shows a button press, but `setValue()` or `postValue()` could be called to update `mName` for a variety of reasons, including in response to a network request or a database load completing; in all cases, the call to `setValue()` or `postValue()` triggers observers and updates the UI.

**Note**: Note: You must call the `setValue(T)` method to update the `LiveData` object from the main thread. If the code is executed in a worker thread, you can use the `postValue(T)` method instead to update the `LiveData` object.

## The advantages of using LiveData
- **Ensures your UI matches your data state**. `LiveData` follows the observer pattern. `LiveData` notifies `Observer` objects when the lifecycle state changes. You can consolidate your code to update the UI in these `Observer` objects. Instead of updating the UI every time the app data changes, your observer can update the UI every time there's a change.
- **No memory leaks**. Observers are bound to `Lifecycle` objects and clean up after themselves when their associated lifecycle is destroyed.
- **No crashes due to stopped activities**. If the observer's lifecycle is inactive, such as in the case of an activity in the back stack, then it doesn’t receive any LiveData events.
- **No more manual lifecycle handling**. UI components just observe relevant data and don’t stop or resume observation. `LiveData` automatically manages all of this since it’s aware of the relevant lifecycle status changes while observing.
- **Proper configuration changes**. If an activity or fragment is recreated due to a configuration change, like device rotation, it immediately receives the latest available data.
- **Sharing resources**. You can extend a `LiveData` object using the singleton pattern to wrap system services so that they can be shared in your app. The `LiveData` object connects to the system service once, and then any observer that needs the resource can just watch the `LiveData` object.

## Links
https://developer.android.com/topic/libraries/architecture/livedata
