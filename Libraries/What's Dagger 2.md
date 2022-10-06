# Dagger 2
Dagger is a fully static, compile-time dependency injection framework for Java, Kotlin, and Android. It is based on the Java Specification Request (JSR) 330. It uses code generation and is based on annotations. It is an adaptation of an earlier version created by Square and now maintained by Google. Dagger aims to address many of the development and performance issues that have plagued reflection-based solutions.

## Dagger annotations

Dagger 2 uses the following annotations:
- `@Module` and `@Provides`: define classes and methods which provide dependencies
- `@Inject`: request dependencies. Can be used on a constructor, a field, or a method
- `@Component`: enable selected modules and used for performing dependency injection

### Defining dependency providers (object providers)
The term dependency injection context is typically used to describe the set of objects which can be injected.

In Dagger 2, classes annotated with `@Module` are responsible for providing objects which can be injected. Such classes can define methods annotated with `@Provides`. The returned objects from these methods are available for dependency injection.

Methods annotated with `@Provides` can also express dependencies via method parameters. These dependencies are fulfilled by Dagger 2, if possible.

### Defining dependencies (object consumers)
You use the `@Inject` annotation to define a dependency. If you annotate a constructor with `@Inject`, Dagger 2 can also use an instance of this object to fulfill dependencies. This was done to avoid the definition of lots of `@Provides` methods for these objects.

### Connecting consumers and providers
The `@Component` is used on an interface. Such an interface is used by Dagger 2 to generate code. The base pattern for the generated class is that Dagger is used as prefix followed by the interface name. This generate class has a create method which allows configuring the objects based on the given configuration. The methods defined on the interface are available to access the generated objects.

A `@Component` interface defines the connection between provider of objects (modules) and the objects which expresses a dependency. The following table gives an overview of the usage of the Dagger annotations.

| Annotation  | Usage |
|---|---|
| `@Module`  |  Used on classes which contains methods annotated with `@Provides`.  |
| `@Provides`  |  Can be used on methods in classes annotated with `@Module` and is used for methods which provides objects for dependencies injection.  |
| `@Singleton`  |  Single instance of this provided object is created and shared.  |
| `@Component`  | Used on an interface. This interface is used by Dagger 2 to generate code which uses the modules to fulfill the requested dependencies. |

## Dagger graph
In Android, you usually create a Dagger graph that lives in your application class because you want an instance of the graph to be in memory as long as the app is running. In this way, the graph is attached to the app lifecycle. In some cases, you might also want to have the application context available in the graph. For that, you would also need the graph to be in the application class. One advantage of this approach is that the graph is available to other Android framework classes. Additionally, it simplifies testing by allowing you to use a custom application class in tests.

When building the Dagger graph for your application:
- When you create a component, you should consider what element is responsible for the lifetime of that component. In this case, the application class was in charge of `ApplicationComponent` and `LoginActivity` in charge of `LoginComponent`.
- Use scoping only when it makes sense. Overusing scoping can have a negative effect on your app's runtime performance: the object is in memory as long as the component is in memory and getting a scoped object is more expensive. When Dagger provides the object, it uses `DoubleCheck` locking instead of a factory-type provider.

## Injection
Because the interface that generates the graph is annotated with `@Component`, you can call it `ApplicationComponent` or `ApplicationGraph`. You usually keep an instance of that component in your custom application class and call it every time you need the application graph, as shown in the following code snippet:

```
// Definition of the Application graph
@Component
interface ApplicationComponent { ... }

// appComponent lives in the Application class to share its lifecycle
class MyApplication: Application() {
    // Reference to the application graph that is used across the whole app
    val appComponent = DaggerApplicationComponent.create()
}
```

Because certain Android framework classes such as activities and fragments are instantiated by the system, Dagger can't create them for you. For activities specifically, any initialization code needs to go into the `onCreate()` method. That means you cannot use the `@Inject` annotation in the constructor of the class (constructor injection) as you did in the previous examples. Instead, you have to use field injection.

Instead of creating the dependencies an activity requires in the `onCreate()` method, you want Dagger to populate those dependencies for you. For field injection, you instead apply the `@Inject` annotation to the fields that you want to get from the Dagger graph.

```
class LoginActivity: Activity() {
    // You want Dagger to provide an instance of LoginViewModel from the graph
    @Inject lateinit var loginViewModel: LoginViewModel
}
```

One of the considerations with Dagger is that injected fields cannot be private. They need to have at least package-private visibility like in the preceding code.

**Note**: Field injection should only be used in Android framework classes where constructor injection cannot be used.

# Links
[Dagger](https://dagger.dev/)

[Using Dagger in Android apps](https://developer.android.com/training/dependency-injection/dagger-android)

[Introduction to Dagger 2](https://www.baeldung.com/dagger-2)

[Using Dagger 2 for dependency injection in Android - Tutorial](https://www.vogella.com/tutorials/Dagger/article.html)

# Next questions
[Main annotations in Dagger 2](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Libraries/Main%20annotations%20in%20Dagger%202.md)

[@Binds vs @Provides](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Libraries/What%20the%20difference%20between%20%40Bind%20and%20%40Provides%20in%20Dagger%202.md)
