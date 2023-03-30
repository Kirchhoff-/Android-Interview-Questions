# Side Effects
A **side-effect** is a change to the state of the app that happens outside the scope of a composable function. Due to composables' lifecycle and properties such as unpredictable recompositions, executing recompositions of composables in different orders, or recompositions that can be discarded, composables should ideally be side-effect free. <sup>[1](https://developer.android.com/jetpack/compose/side-effects#:~:text=A%20side%2Deffect,effect%20free.)</sup>

However, sometimes side-effects are necessary, for example, to trigger a one-off event such as showing a snackbar or navigate to another screen given a certain state condition. These actions should be called from a controlled environment that is aware of the lifecycle of the composable. <sup>[2](https://developer.android.com/jetpack/compose/side-effects#:~:text=However%2C%20sometimes%20side%2Deffects%20are%20necessary%2C%20for%20example%2C%20to%20trigger%20a%20one%2Doff%20event%20such%20as%20showing%20a%20snackbar%20or%20navigate%20to%20another%20screen%20given%20a%20certain%20state%20condition.%20These%20actions%20should%20be%20called%20from%20a%20controlled%20environment%20that%20is%20aware%20of%20the%20lifecycle%20of%20the%20composable.)</sup>

## [State and effect use cases](https://developer.android.com/jetpack/compose/side-effects#state-effect-use-cases)

### [LaunchedEffect: run suspend functions in the scope of a composable](https://developer.android.com/jetpack/compose/side-effects#launchedeffect)
To call suspend functions safely from inside a composable, use the `LaunchedEffect` composable. When `LaunchedEffect` enters the Composition, it launches a coroutine with the block of code passed as a parameter. The coroutine will be cancelled if `LaunchedEffect` leaves the composition. If `LaunchedEffect` is recomposed with different keys, the existing coroutine will be cancelled and the new suspend function will be launched in a new coroutine.

For example, showing a `Snackbar` in a `Scaffold` is done with the `SnackbarHostState.showSnackbar` function, which is a suspend function.
```
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MyScreen(
    state: UiState<List<Movie>>,
    snackbarHostState: SnackbarHostState
) {

    // If the UI state contains an error, show snackbar
    if (state.hasError) {

        // `LaunchedEffect` will cancel and re-launch if
        // `scaffoldState.snackbarHostState` changes
        LaunchedEffect(snackbarHostState) {
            // Show snackbar using a coroutine, when the coroutine is cancelled the
            // snackbar will automatically dismiss. This coroutine will cancel whenever
            // `state.hasError` is false, and only start when `state.hasError` is true
            // (due to the above if-check), or if `scaffoldState.snackbarHostState` changes.
            snackbarHostState.showSnackbar(
                message = "Error message",
                actionLabel = "Retry message"
            )
        }
    }

    Scaffold(
        snackbarHost = {
            SnackbarHost(hostState = snackbarHostState)
        }
    ) { contentPadding ->
        // ...
    }
}
```

In the code above, a coroutine is triggered if the state contains an error and it'll be cancelled when it doesn't. As the `LaunchedEffect` call site is inside an if statement, when the statement is false, if `LaunchedEffect` was in the Composition, it'll be removed, and therefore, the coroutine will be cancelled.

### [rememberCoroutineScope: obtain a composition-aware scope to launch a coroutine outside a composable](https://developer.android.com/jetpack/compose/side-effects#remembercoroutinescope)
As `LaunchedEffect` is a composable function, it can only be used inside other composable functions. In order to launch a coroutine outside of a composable, but scoped so that it will be automatically canceled once it leaves the composition, use `rememberCoroutineScope`. Also use `rememberCoroutineScope` whenever you need to control the lifecycle of one or more coroutines manually, for example, cancelling an animation when a user event happens.

`rememberCoroutineScope` is a composable function that returns a `CoroutineScope` bound to the point of the Composition where it's called. The scope will be cancelled when the call leaves the Composition.

Following the previous example, you could use this code to show a `Snackbar` when the user taps on a `Button`:
```
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MoviesScreen(snackbarHostState: SnackbarHostState) {

    // Creates a CoroutineScope bound to the MoviesScreen's lifecycle
    val scope = rememberCoroutineScope()

    Scaffold(
        snackbarHost = {
            SnackbarHost(hostState = snackbarHostState)
        }
    ) { contentPadding ->
        Column(Modifier.padding(contentPadding)) {
            Button(
                onClick = {
                    // Create a new coroutine in the event handler to show a snackbar
                    scope.launch {
                        snackbarHostState.showSnackbar("Something happened!")
                    }
                }
            ) {
                Text("Press me")
            }
        }
    }
}
```

### [rememberUpdatedState: reference a value in an effect that shouldn't restart if the value changes](https://developer.android.com/jetpack/compose/side-effects#rememberupdatedstate)
`LaunchedEffect` restarts when one of the key parameters changes. However, in some situations you might want to capture a value in your effect that, if it changes, you do not want the effect to restart. In order to do this, it is required to use `rememberUpdatedState` to create a reference to this value which can be captured and updated. This approach is helpful for effects that contain long-lived operations that may be expensive or prohibitive to recreate and restart.

For example, suppose your app has a LandingScreen that disappears after some time. Even if LandingScreen is recomposed, the effect that waits for some time and notifies that the time passed shouldn't be restarted:
```
@Composable
fun LandingScreen(onTimeout: () -> Unit) {

    // This will always refer to the latest onTimeout function that
    // LandingScreen was recomposed with
    val currentOnTimeout by rememberUpdatedState(onTimeout)

    // Create an effect that matches the lifecycle of LandingScreen.
    // If LandingScreen recomposes, the delay shouldn't start again.
    LaunchedEffect(true) {
        delay(SplashWaitTimeMillis)
        currentOnTimeout()
    }

    /* Landing screen content */
}
```

To create an effect that matches the lifecycle of the call site, a never-changing constant like `Unit` or `true` is passed as a parameter. In the code above, `LaunchedEffect(true)` is used. To make sure that the `onTimeout` lambda *always* contains the latest value that `LandingScreen` was recomposed with, onTimeout needs to be wrapped with the `rememberUpdatedState` function. The returned `State`, `currentOnTimeout` in the code, should be used in the effect.

### [DisposableEffect: effects that require cleanup](https://developer.android.com/jetpack/compose/side-effects#disposableeffect)
For side effects that need to be *cleaned up* after the keys change or if the composable leaves the Composition, use `DisposableEffect`. If the `DisposableEffect` keys change, the composable needs to *dispose* (do the cleanup for) its current effect, and reset by calling the effect again.

As an example, you might want to send analytics events based on `Lifecycle` events by using a `LifecycleObserver`. To listen for those events in Compose, use a `DisposableEffect` to register and unregister the observer when needed.
```
@Composable
fun HomeScreen(
    lifecycleOwner: LifecycleOwner = LocalLifecycleOwner.current,
    onStart: () -> Unit, // Send the 'started' analytics event
    onStop: () -> Unit // Send the 'stopped' analytics event
) {
    // Safely update the current lambdas when a new one is provided
    val currentOnStart by rememberUpdatedState(onStart)
    val currentOnStop by rememberUpdatedState(onStop)

    // If `lifecycleOwner` changes, dispose and reset the effect
    DisposableEffect(lifecycleOwner) {
        // Create an observer that triggers our remembered callbacks
        // for sending analytics events
        val observer = LifecycleEventObserver { _, event ->
            if (event == Lifecycle.Event.ON_START) {
                currentOnStart()
            } else if (event == Lifecycle.Event.ON_STOP) {
                currentOnStop()
            }
        }

        // Add the observer to the lifecycle
        lifecycleOwner.lifecycle.addObserver(observer)

        // When the effect leaves the Composition, remove the observer
        onDispose {
            lifecycleOwner.lifecycle.removeObserver(observer)
        }
    }

    /* Home screen content */
}
```
In the code above, the effect will add the `observer` to the `lifecycleOwner`. If `lifecycleOwner` changes, the effect is disposed and restarted with the new `lifecycleOwner`.

### [SideEffect: publish Compose state to non-compose code](https://developer.android.com/jetpack/compose/side-effects#sideeffect-publish)
To share Compose state with objects not managed by compose, use the `SideEffect` composable, as it's invoked on every successful recomposition.

For example, your analytics library might allow you to segment your user population by attaching custom metadata ("user properties" in this example) to all subsequent analytics events. To communicate the user type of the current user to your analytics library, use `SideEffect` to update its value.
```
@Composable
fun rememberFirebaseAnalytics(user: User): FirebaseAnalytics {
    val analytics: FirebaseAnalytics = remember {
        FirebaseAnalytics()
    }

    // On every successful composition, update FirebaseAnalytics with
    // the userType from the current User, ensuring that future analytics
    // events have this metadata attached
    SideEffect {
        analytics.setUserProperty("userType", user.userType)
    }
    return analytics
}
```

### [produceState: convert non-Compose state into Compose state](https://developer.android.com/jetpack/compose/side-effects#producestate)
`produceState` launches a coroutine scoped to the Composition that can push values into a returned `State`.  Use it to convert non-Compose state into Compose state, for example bringing external subscription-driven state such as `Flow`, `LiveData`, or `RxJava` into the Composition.

The producer is launched when `produceState` enters the Composition, and will be cancelled when it leaves the Composition. The returned `State` conflates; setting the same value won't trigger a recomposition.

The following example shows how to use `produceState` to load an image from the network. The `loadNetworkImage` composable function returns a `State` that can be used in other composables.
```
@Composable
fun loadNetworkImage(
    url: String,
    imageRepository: ImageRepository = ImageRepository()
): State<Result<Image>> {

    // Creates a State<T> with Result.Loading as initial value
    // If either `url` or `imageRepository` changes, the running producer
    // will cancel and will be re-launched with the new inputs.
    return produceState<Result<Image>>(initialValue = Result.Loading, url, imageRepository) {

        // In a coroutine, can make suspend calls
        val image = imageRepository.load(url)

        // Update State with either an Error or Success result.
        // This will trigger a recomposition where this State is read
        value = if (image == null) {
            Result.Error
        } else {
            Result.Success(image)
        }
    }
}
```

### [derivedStateOf: convert one or multiple state objects into another state](https://developer.android.com/jetpack/compose/side-effects#derivedstateof)
Use `derivedStateOf` when a certain state is calculated or derived from other state objects. Using this function guarantees that the calculation will only occur whenever one of the states used in the calculation changes.

The following example shows a basic To Do list whose tasks with user-defined high priority keywords appear first:
```
@Composable
fun TodoList(highPriorityKeywords: List<String> = listOf("Review", "Unblock", "Compose")) {

    val todoTasks = remember { mutableStateListOf<String>() }

    // Calculate high priority tasks only when the todoTasks or highPriorityKeywords
    // change, not on every recomposition
    val highPriorityTasks by remember(highPriorityKeywords) {
        derivedStateOf {
            todoTasks.filter { task ->
                highPriorityKeywords.any { keyword ->
                    task.contains(keyword)
                }
            }
        }
    }

    Box(Modifier.fillMaxSize()) {
        LazyColumn {
            items(highPriorityTasks) { /* ... */ }
            items(todoTasks) { /* ... */ }
        }
        /* Rest of the UI where users can add elements to the list */
    }
}
```
In the code above, `derivedStateOf` guarantees that whenever `todoTasks` changes, the `highPriorityTasks` calculation will occur and the UI will be updated accordingly. If `highPriorityKeywords` changes, the `remember` block will be executed and a new derived state object will be created and remembered in place of the old one. As the filtering to calculate `highPriorityTasks` can be expensive, it should only be executed when any of the lists change, not on every recomposition.

# Links  
[Side-effects in Compose](https://developer.android.com/jetpack/compose/side-effects)

# Further Reading
[Jetpack Compose Side Effects Made Easy](https://medium.com/mobile-app-development-publication/jetpack-compose-side-effects-made-easy-a4867f876928)

[Everything you need to know about Side Effects in Jetpack Compose with examples](https://www.composables.co/tutorials/side-effects)

[Jetpack Compose Side Effects — LaunchedEffect With Example](https://betterprogramming.pub/jetpack-compose-side-effects-launchedeffect-with-example-99c2f51ff463)
