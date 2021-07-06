# App Startup
The App Startup library provides a straightforward, performant way to initialize components at application startup. Both library developers and app developers can use App Startup to streamline startup sequences and explicitly set the order of initialization.

Instead of defining separate content providers for each component you need to initialize, App Startup allows you to define component initializers that share a single content provider. This can significantly improve app startup time.

## [Initialize components at app startup](https://developer.android.com/topic/libraries/app-startup#startup-initialize)
Apps and libraries often rely on having components initialized right away when the app starts up. You can meet this need by using content providers to initialize each dependency, but content providers are expensive to instantiate and can slow down the startup sequence unnecessarily. Additionally, Android initializes content providers in an undetermined order. App Startup provides a more performant way to initialize components at app startup and explicitly define their dependencies.

To use App Startup to initialize components automatically at startup, you must define a component initializer for each component that the app needs to initialize.

## [Implement component initializers](https://developer.android.com/topic/libraries/app-startup#implement-initializers)
You define each component initializer by creating a class that implements the `Initializer<T>` interface. This interface defines two important methods:

- The `create()` method, which contains all of the necessary operations to initialize the component and returns an instance of `T`.
- The `dependencies()` method, which returns a list of the other `Initializer<T>` objects that the initializer depends on. You can use this method to control the order in which the app runs the initializers at startup.

For example, suppose that your app depends on `WorkManager` and needs to initialize it at startup. Define a `WorkManagerInitializer` class that implements `Initializer<WorkManager>`:

```
// Initializes WorkManager.
class WorkManagerInitializer : Initializer<WorkManager> {
    override fun create(context: Context): WorkManager {
        val configuration = Configuration.Builder().build()
        WorkManager.initialize(context, configuration)
        return WorkManager.getInstance(context)
    }
    
    override fun dependencies(): List<Class<out Initializer<*>>> {
        // No dependencies on other libraries.
        return emptyList()
    }
}
```

The `dependencies()` method returns an empty list because `WorkManager` does not depend on any other libraries.

Suppose that your app also depends on a library called `ExampleLogger`, which in turn depends on `WorkManager`. This dependency means that you need to make sure that App Startup initializes `WorkManager` first. Define an `ExampleLoggerInitializer` class that implements `Initializer<ExampleLogger>`:

```
// Initializes ExampleLogger.
class ExampleLoggerInitializer : Initializer<ExampleLogger> {
    override fun create(context: Context): ExampleLogger {
        // WorkManager.getInstance() is non-null only after
        // WorkManager is initialized.
        return ExampleLogger(WorkManager.getInstance(context))
    }

    override fun dependencies(): List<Class<out Initializer<*>>> {
        // Defines a dependency on WorkManagerInitializer so it can be
        // initialized after WorkManager is initialized.
        return listOf(WorkManagerInitializer::class.java)
    }
}
```

Because you include `WorkManagerInitializer` in the `dependencies()` method, App Startup initializes `WorkManager` before `ExampleLogger`.

## [Set up manifest entries](https://developer.android.com/topic/libraries/app-startup#manifest-entries)
App Startup includes a special content provider called `InitializationProvider` that it uses to discover and call your component initializers. App Startup discovers component initializers by first checking for a `<meta-data>` entry under the `InitializationProvider` manifest entry. Then, App Startup calls the `dependencies()` methods for any initializers that it has already discovered.

This means that in order for a component initializer to be discoverable by App Startup, one of the following conditions must be met:
- The component initializer has a corresponding `<meta-data>` entry under the `InitializationProvider` manifest entry.
- The component initializer is listed in the `dependencies()` method from an initializer that is already discoverable.

Consider again the example with `WorkManagerInitializer` and `ExampleLoggerInitializer`. To make sure App Startup can discover these initializers, add the following to the manifest file:
```
<provider
    android:name="androidx.startup.InitializationProvider"
    android:authorities="${applicationId}.androidx-startup"
    android:exported="false"
    tools:node="merge">
    <!-- This entry makes ExampleLoggerInitializer discoverable. -->
    <meta-data  android:name="com.example.ExampleLoggerInitializer"
          android:value="androidx.startup" />
</provider>
```

You don't need to add a `<meta-data>` entry for `WorkManagerInitializer`, because `WorkManagerInitializer` is a dependency of `ExampleLoggerInitializer`. This means that if `ExampleLoggerInitializer` is discoverable, then so is `WorkManagerInitializer`.

## [Manually initialize components](https://developer.android.com/topic/libraries/app-startup#manual)
Ordinarily when you use App Startup, the `InitializationProvider` object uses an entity called `AppInitializer` to automatically discover and run component initializers at application startup. However, you can also use `AppInitializer` directly in order to manually initialize components that your app doesn't need at startup. This is called *lazy initialization*, and it can help minimize startup costs.

You must first disable automatic initialization for any components that you want to initialize manually.

## [Manually call component initializers](https://developer.android.com/topic/libraries/app-startup#manual-initialization)
If automatic initialization is disabled for a component, you can use `AppInitializer` to manually initialize that component and its dependencies.

For example, the following code calls `AppInitializer` and manually initializes `ExampleLogger`:

```
AppInitializer.getInstance(context)
    .initializeComponent(ExampleLoggerInitializer::class.java)
```

As a result, App Startup also initializes `WorkManager` because `WorkManager` is a dependency of `ExampleLogger`.

# Links
[App Startup](https://developer.android.com/topic/libraries/app-startup)
