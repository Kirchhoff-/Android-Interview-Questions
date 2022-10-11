# Kodein 
Kodein is a *Dependency Injection* library. It allows you to bind your business unit interfaces with their implementation and thus having each business unit being independent.

*Kodein-DI* allows you to:
- Lazily instantiate your dependencies when needed;
- Stop caring about dependency initialization order;
- Easily bind classes or interfaces to their instance, provider or factory;
- Easily debug your dependency bindings and recursions.

*Kodein-DI* is a good choice because:
- It is small, fast and optimized (makes extensive use of `inline`);
- It proposes a very simple and readable declarative DSL;
- It is not subject to type erasure (like Java);
- It integrates nicely with Android;
- It proposes a very kotlin-esque idiomatic API;
- It (also) can be used in plain Java.

*Kodein* is compatible with all platforms that the Kotlin language compiles to: JVM & compatible (Android), Javascript and all the Kotlin/Native targets.

## [Bindings](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_bindings)

### [Declaration](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_declaration)
In Kodein, bindings are **declared** in a DI Block. The syntax is quite simple:
```
val kodein = DI {
    bindProvider<Random> { SecureRandom() }
    bindSingleton<Database> { SQLiteDatabase() }
}
```
As you can see, Kodein offers a DSL (Domain Specific Language) that allows to very easily declare a binding. Kodein offers many bindings that can manage implementations: `provider`, `singleton`, `factory`, `multiton`, `instance`, and more.

### [Separation](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_separation)
When building large applications, there are often different modules, built by their own team, each defining their own business units.

Kodein allows you to define binding modules that can later be imported in a DI container:
```
val module = DI.Module {
    bindSingleton<Database>(tag = "local") { SQLiteDatabase() }
    bindProvider<Database>(tag = "remote") { DatabaseProxy() }
}
```

Importing a DI module:
```
val di = DI {
    import(module)
}
```

## [Injection & Retrieval](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_injection_retrieval)

### [The container](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_the_container)
All declarations are made in the constructor of a DI **container**. Think of the DI container as the glue that allows you to ask for dependency. Whatever dependency you need, if it was declared in the DI container constructor, you can get it by asking DI. This means that you always need to be within reach of the `DI` object. There are multiple ways of doing so:
- You can pass the `DI` object around, as a parameter to created objects;
- You can have the `DI` object being statically available (in Android, for example, it is common to use a property of the `Application` object).

### [Injection vs Retrieval](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_injection_vs_retrieval)
Kodein supports two different methods to allow a business unit to access its dependencies: **injection** and **retrieval**.
- When dependencies are **injected**, the class is **provided** its dependencies at **construction**;
- When dependencies are **retrieved**, the class is **responsible** for **getting** its own dependencies.

#### [Injection](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_injection)
If you want your class to be *injected*, then you need to declare its dependencies at construction:
```
class Presenter(private val db: Database, private val rnd: Random) {}
```

Now you need to be able to create a new instance of this `Presenter` class. With Kodein-DI, this is very easy:
```
val presenter by di.newInstance { Presenter(instance(), instance()) }
```

For each argument of the `Presenter` constructor, you can simply use the `instance()` function, and Kodein-DI will actually pass the correct dependency.

#### [Retrieval](https://kosi-libs.org/kodein/7.14.0/getting-started.html#_retrieval)
When using retrieval, the class needs to be available to access a DI instance, either statically or by argument. In these examples, we’ll use the argument.
```
class Presenter(val di: DI) {
    private val db: Database by di.instance()
    private val rnd: Random by di.instance()
}
```

Note the usage of the `by` keyword. When using dependency retrieval, **properties are retrieved lazily**.

## [Kodein on Android](https://kosi-libs.org/kodein/7.14.0/framework/android.html)
You can use Kodein as-is in your Android project or use the util library `kodein-di-android`.

**Note**. Kodein does work on Android as-is. The `kodein-di-android-*` extensions add multiple android-specific utilities to Kodein.
Using or not using this extension really depends on your needs.

### [Install](https://kosi-libs.org/kodein/7.14.0/framework/android.html#install)
How to use `kodein-di-android`:
1. Add this line in your `dependencies` block in your application `build.gradle` file:
```
implementation 'org.kodein.di:kodein-di-framework-android-???:7.14.0'
```

2. Declare the dependency bindings in the Android `Application`, having it implements `DIAware`.
```
class MyApp : Application(), DIAware {
	override val di by DI.lazy { 
	    /* bindings */
	}
}
```

3. In your Activities, Fragments, and other context aware android classes, retrieve the `DI` object with the `di` function.
4. Retrieve your dependencies!

### [View Models](https://kosi-libs.org/kodein/7.14.0/framework/android.html#_view_models)

The `kodein-di-framework-android-x-viewmodel` module simplifies injection and retrieval of View Models which inherit `androidx.lifecycle.ViewModel`, without requiring that the View Model implement `DIAware`.

**Note**. If your View Model uses a `SavedStateHandle`, use the [View Model with SavedStateHandle module instead](https://kosi-libs.org/kodein/7.14.0/framework/android.html#view-models-saved-state-handle).

Create a binding for a ViewModel:
```
val di = DI {
    bindProvider { MyViewModel(
        myDependency = instance()
    ) }
}
```

Retrieve a plain `ViewModel` in an `Activity` or `Fragment`:
```
class MyActivity : AppCompatActivity(), DIAware {
    override val di: DI by closestDI()
    private val viewModel: MyViewModel by viewModel()
}

class MyFragment : Fragment(), DIAware {
    override val di: DI by closestDI()
    private val viewModel: MyViewModel by viewModel()
}
```

# Links
[Kodein](https://github.com/kosi-libs/Kodein)

[Kodein Open Source Initiative documentation](https://kosi-libs.org/kosi-libs/index.html)

# Further reading
[Kotlin Dependency Injection with Kodein](https://www.baeldung.com/kotlin/kodein-dependency-injection)
