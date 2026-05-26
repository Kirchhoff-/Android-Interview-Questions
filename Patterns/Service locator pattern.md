# Service locator pattern
The Service Locator Pattern is a design pattern used to manage object dependencies by providing a centralized registry, or "locator," from which objects or services can be fetched as needed. Instead of injecting dependencies directly into a class, the class retrieves them from the service locator, creating a form of indirect dependency management.<sup>[1](https://blog.kotzilla.io/is-koin-a-dependency-injection-framework-or-a-service-locator#:~:text=The%20Service%20Locator%20Pattern%20is,form%20of%20indirect%20dependency%20management.)</sup>

At a high level, the pattern usually works like this:
- Create a globally accessible singleton object;
- Store shared dependencies inside it;
- Let classes request those dependencies directly whenever needed.
- Reuse the same instances across the application.

Example:
```kotlin
object ServiceLocator {

    val apiService: ApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .build()
            .create(ApiService::class.java)
    }

    val userRepository: UserRepository by lazy {
        UserRepository(apiService)
    }
}
```

Usage:

```kotlin
class UserViewModel : ViewModel() {

    private val userRepository = ServiceLocator.userRepository

    fun loadUser() {
        userRepository.loadUser()
    }
}
```

Instead of receiving dependencies from the outside, the class fetches them directly from the Service Locator whenever needed.

## What is the main problem with Service Locator?
The main issue with the Service Locator pattern is **hidden dependencies**.

At first glance, the `UserViewModel` above looks simple:
```kotlin
class UserViewModel : ViewModel() {

    private val userRepository = ServiceLocator.userRepository
}
```

But this class is not actually self-contained. It depends on `UserRepository`, but that dependency is not visible in the constructor or public API. Instead, it is hidden inside the implementation.

This creates several problems:
- harder to understand what the class actually depends on;
- harder to test, because dependencies are coupled to global state;
- tighter coupling between business logic and dependency creation;
- reduced flexibility when replacing implementations.

Compare this with Dependency Injection:
```kotlin
class UserViewModel(
    private val userRepository: UserRepository,
) : ViewModel()
```

Here, the dependency is explicit. You can immediately see what the class needs to work.

## Testing problems

Testing becomes more difficult because dependencies are tied to global state.

For example:

```kotlin
object ServiceLocator {
    val userRepository = UserRepository()
}
```

Now every test that uses `UserViewModel` implicitly depends on whatever `ServiceLocator` returns.

Replacing that dependency often requires:

- modifying global state;
- adding test-specific hooks;
- resetting shared objects between tests.

This increases test complexity and can make tests more fragile and less isolated.

## Android examples

Some Android dependency management solutions can resemble the Service Locator pattern depending on how they are used.

For example:

### Koin

```kotlin
class UserViewModel : ViewModel(), KoinComponent {

    private val userRepository: UserRepository by inject()

    fun loadUser() {
        userRepository.loadUser()
    }
}
```

Here, the dependency is requested directly from the container inside the class.

This feels convenient, but the dependency is still hidden from the constructor and not immediately visible from the public API.

### Kodein

```kotlin
class UserViewModel(
    override val di: DI,
) : ViewModel(), DIAware {

    private val userRepository: UserRepository by instance()

    fun loadUser() {
        userRepository.loadUser()
    }
}
```

This follows a very similar idea.

Instead of receiving `UserRepository` directly, the class asks the dependency container to provide it.

### Important note

This does **not** automatically mean that frameworks like Koin or Kodein are bad or that they should be avoided. Both frameworks also support cleaner dependency management approaches.

The key distinction is not the framework itself, but how dependencies are obtained. If a class requests dependencies directly from a container, that approach starts to resemble the Service Locator pattern.

## Why do developers still use Service Locator?
Despite its drawbacks, the Service Locator pattern can look attractive for several reasons:

- quick to set up;
- no need to pass dependencies through multiple constructors;
- easy to understand in small projects;
- dependencies are accessible from anywhere;
- can feel simpler than introducing a DI framework.

In small prototypes, throwaway projects, or legacy codebases, Service Locator may be a pragmatic compromise.

## Conclusion

The Service Locator pattern can feel like a practical and straightforward solution, especially in smaller projects where speed and simplicity matter more than architectural purity.

However, like many convenient patterns, its trade-offs tend to become much more visible as a codebase grows.

**Pros**:
- simple to understand;
- quick to set up;
- easy access to shared dependencies;
- less boilerplate in small projects;
- can be practical in prototypes or legacy codebases.

**Cons**:
- hidden dependencies;
- tighter coupling to global state;
- harder testing and less isolated tests;
- reduced flexibility when replacing implementations;
- architecture becomes harder to understand as the project grows.

# Links
[Is Koin a Dependency Injection Framework or a Service Locator?](https://blog.kotzilla.io/is-koin-a-dependency-injection-framework-or-a-service-locator)

## Next questions

[What do you know about DI?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20do%20you%20know%20about%20DI.md)

[What is the difference between DI and Service Locator?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20is%20the%20difference%20between%20DI%20and%20Service%20Locator.md)

[Which libraries for dependency injection you know?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Libraries/Which%20libraries%20for%20dependency%20injection%20you%20know.md)

# Further reading
[Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html)
