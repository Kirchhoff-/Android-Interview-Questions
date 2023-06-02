# `ViewCompositionStrategy`
[`ViewCompositionStrategy`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ViewCompositionStrategy) defines when the Composition should be disposed. The default, [`ViewCompositionStrategy.Default`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ViewCompositionStrategy#Default()), disposes the Composition when the underlying [`ComposeView`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ComposeView) detaches from the window, unless it is part of a pooling container such as a `RecyclerView`. In a single-Activity Compose-only app, this default behavior is what you would want, however, if you are incrementally adding Compose in your codebase, this behavior may cause state loss in some scenarios.

To change the `ViewCompositionStrategy`, call the [`setViewCompositionStrategy()`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/AbstractComposeView#setViewCompositionStrategy(androidx.compose.ui.platform.ViewCompositionStrategy)) method and provide a different strategy.

There are four different options for `ViewCompositionStrategy`:
- **[`DisposeOnDetachedFromWindow`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ViewCompositionStrategy.DisposeOnDetachedFromWindow)**. The Composition will be disposed when the underlying `ComposeView` is detached from the window. Has since been superseded by `DisposeOnDetachedFromWindowOrReleasedFromPool`. 
  
  Interop scenario:
  
  * `ComposeView` whether it’s the sole element in the View hierarchy, or in the context of a mixed View/Compose screen (not in Fragment).

- **[`DisposeOnDetachedFromWindowOrReleasedFromPool`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ViewCompositionStrategy.DisposeOnDetachedFromWindowOrReleasedFromPool) (Default)**. Similar to `DisposeOnDetachedFromWindow`, when the Composition is not in a pooling container, such as a `RecyclerView`. If it is in a pooling container, it will dispose when either the pooling container itself detaches from the window, or when the item is being discarded (i.e. when the pool is full).

  Interop scenario:
  
  * `ComposeView` whether it's the sole element in the View hierarchy, or in the context of a mixed View/Compose screen (not in Fragment);
  * `ComposeView` as an item in a pooling container such as `RecyclerView`.

- **[`DisposeOnLifecycleDestroyed`](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ViewCompositionStrategy.DisposeOnLifecycleDestroyed)**. The Composition will be disposed when the provided [`Lifecycle`](https://developer.android.com/reference/androidx/lifecycle/Lifecycle) is destroyed.

  Interop scenario:
  
  * `ComposeView` in a Fragment's View.

- **[DisposeOnViewTreeLifecycleDestroyed](https://developer.android.com/reference/kotlin/androidx/compose/ui/platform/ViewCompositionStrategy.DisposeOnViewTreeLifecycleDestroyed)**. The Composition will be disposed when the `Lifecycle` owned by the `LifecycleOwner` returned by `ViewTreeLifecycleOwner.get` of the next window the View is attached to is destroyed.

  Interop scenario:
  
  * `ComposeView` in a Fragment's View;
  * `ComposeView` in a View wherein the Lifecycle is not known yet.

# Links
[Using Compose in Views](https://developer.android.com/jetpack/compose/migrate/interoperability-apis/compose-in-views)

# Further Reading
[`ViewCompositionStrategy` Demystified](https://medium.com/androiddevelopers/viewcompositionstrategy-demystefied-276427152f34)
