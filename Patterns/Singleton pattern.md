# Singleton pattern

Singleton is a design pattern that ensures only one instance of a class exists and provides a global access point to that instance.

In Kotlin, the `object` keyword provides a built-in way to declare a singleton. The instance is created automatically and can be accessed from anywhere in the application.

Singleton pattern is best used when:
- Only one instance of a class should exist throughout the application
- Multiple parts of the application need access to the same shared resource
- Creating multiple instances would be unnecessary, wasteful, or expensive

Common Android examples include:
- Network clients (`Retrofit`, `OkHttpClient`)
- Database instances (`RoomDatabase`)
- Analytics and tracking services
- Configuration providers
- Preference storage wrappers

## Singleton declaration
As mentioned earlier, Kotlin provides built-in support for the Singleton pattern through the `object` keyword.

The following example demonstrates how a singleton can be declared and accessed throughout the application:<sup>[1](https://kotlinlang.org/docs/object-declarations.html#object-declarations-overview:~:text=implementing%20singletons%3A)</sup>
```kotlin
// Declares a Singleton object to manage data providers
object DataProviderManager {
    private val providers = mutableListOf<DataProvider>()

    // Registers a new data provider
    fun registerDataProvider(provider: DataProvider) {
        providers.add(provider)
    }

    // Retrieves all registered data providers
    val allDataProviders: Collection<DataProvider> 
        get() = providers
}
```

The initialization of an object declaration is thread-safe and done on first access.

To refer to the object, use its name directly:<sup>[2](https://kotlinlang.org/docs/object-declarations.html#object-declarations-overview:~:text=The%20initialization%20of,(exampleProvider))</sup>
```kotlin
DataProviderManager.registerDataProvider(exampleProvider)
```

## Android examples

### Analytics service

```kotlin
object AnalyticsTracker {

    fun trackEvent(eventName: String) {
        println("Event tracked: $eventName")
    }
}
```

Analytics services are often implemented as singletons because multiple parts of the application need to report events through the same shared instance.

### Preference storage

```kotlin
object PreferenceStorage {

    private val preferences = mutableMapOf<String, String>()

    fun saveToken(token: String) {
        preferences["token"] = token
    }

    fun getToken(): String? {
        return preferences["token"]
    }
}
```

Preference storage is commonly implemented as a singleton because application settings and user data need to be accessible from different parts of the application.

### Configuration provider

```kotlin
object FeatureFlags {

    const val isNewProfileEnabled = true
    const val isNewSearchEnabled = false
}
```

Configuration providers are often implemented as singletons because feature flags and application settings should have a single source of truth.

## Singleton and Dependency Injection

Singleton and Dependency Injection solve different problems.

The Singleton pattern controls an object's lifetime and ensures that only one instance exists.

Dependency Injection, on the other hand, controls how dependencies are provided to a class.

These concepts are often used together. For example, a Dependency Injection framework can create and manage singleton-scoped objects:

```kotlin
single { Retrofit.Builder().build() }

single { OkHttpClient() }

single { UserRepository(get()) }
```

In this example, the DI container is responsible for creating the objects and ensuring that only one instance exists throughout the application.

While both approaches can provide a single shared instance, Dependency Injection makes dependencies explicit.

Classes receive their dependencies through constructors rather than accessing global objects directly.

This improves testability, reduces coupling, and makes dependencies easier to understand and maintain.


## Why Singleton can be difficult to test

One of the most common criticisms of the Singleton pattern is that it can make testing more difficult.

Consider the following example:

```kotlin
class LoginViewModel {

    fun login() {
        AnalyticsTracker.trackEvent("Login")
    }
}
```

In this case, `LoginViewModel` depends directly on the global `AnalyticsTracker` instance.

This creates a hidden dependency because the dependency is not visible in the constructor and cannot be easily replaced during testing.

A more testable approach is to inject the dependency:

```kotlin
class LoginViewModel(
    private val analyticsTracker: AnalyticsTracker
) {

    fun login() {
        analyticsTracker.trackEvent("Login")
    }
}
```

Now the dependency is explicit and can be replaced with a fake or mock implementation during tests.

For this reason, modern Android applications often prefer Dependency Injection over direct access to singleton objects.

## Singleton and memory leaks

Singletons typically live for the entire lifetime of the application.

Because of this, developers should be careful when storing references to Android components such as `Activity`, `Fragment`, or `View`.

Consider the following example:

```kotlin
object AnalyticsTracker {

    var activity: Activity? = null
}
```

If the `Activity` is destroyed but the singleton still holds a reference to it, the garbage collector cannot release that memory.

This can result in a memory leak.

For this reason, singleton objects should generally avoid storing references to short-lived Android components.

## Pros and cons

Pros:
- Ensures only one instance of an object exists;
- Reduces unnecessary object creation;
- Provides a centralized access point to shared resources;
- Can simplify resource management for application-wide services.

Cons:
- Can introduce hidden dependencies;
- Makes testing more difficult when accessed globally;
- May lead to excessive global state if overused;
- Can contribute to memory leaks when holding references to short-lived Android components;
- Can increase coupling between components.

# Links
[Object declarations and expressions](https://kotlinlang.org/docs/object-declarations.html)

# Further reading
[Singleton Design Pattern](https://sourcemaking.com/design_patterns/singleton)

[Singleton](https://refactoring.guru/design-patterns/singleton)

[Singleton Software Pattern Kotlin Examples](https://softwarepatterns.com/en/singleton-software-pattern/singleton-software-pattern-kotlin-example)
