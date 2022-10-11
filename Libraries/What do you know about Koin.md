# Koin 
A pragmatic lightweight dependency injection framework for Kotlin developers. Written in pure Kotlin, using functional resolution only: no proxy, no code generation, no reflection. Koin is a DSL, a lightweight container and a pragmatic API.

## [Koin DSL](https://insert-koin.io/docs/reference/koin-core/dsl)

### [Application & Module DSL](https://insert-koin.io/docs/reference/koin-core/dsl#application--module-dsl)
Koin offers several keywords to let you describe the elements of a Koin Application:
- Application DSL, to describe the Koin container configuration;
- Module DSL, to describe the components that have to be injected.

### [Application DSL](https://insert-koin.io/docs/reference/koin-core/dsl#application-dsl)
A `KoinApplication` instance is a Koin container instance configuration. This will let your configure logging, properties loading and modules.

To build a new `KoinApplication`, use the following functions:
- `koinApplication { }` - create a `KoinApplication` container configuration;
- `startKoin { }` - create a `KoinApplication` container configuration and register it in the GlobalContext to allow the use of GlobalContext API;

### [Module DSL](https://insert-koin.io/docs/reference/koin-core/dsl#module-dsl)
A Koin module gather definitions that you will inject/combine for your application. To create a new module, just use the following function:

- `module { // module content }` - create a Koin Module

To describe your content in a module, you can use the following functions:
- `factory { //definition }` - provide a factory bean definition;
- `single { //definition }` - provide a singleton bean definition (also aliased as bean);
- `get()` - resolve a component dependency (also can use name, scope or parameters);
- `bind()` - add type to bind for given bean definition;
- `binds()` - add types array for given bean definition;
- `scope { // scope group }` - define a logical group for scoped definition;
- `scoped { //definition }` - provide a bean definition that will exists only in a scope.

**Note**: the `named()` function allow you to give a qualifier either by a string, an enum or a type. It is used to name your definitions.

### [Writing a module](https://insert-koin.io/docs/reference/koin-core/dsl#writing-a-module)
A Koin module is the *space to declare all your components*. Use the `module` function to declare a Koin module:
```
val myModule = module {
   // your dependencies here
}
```

## [Starting Koin on Android](https://insert-koin.io/docs/reference/koin-android/start/)

### [startKoin() from your Application](https://insert-koin.io/docs/reference/koin-android/start/#startkoin-from-your-application)
From your `Application` class you can use the `startKoin` function and inject the Android context with `androidContext` as follow:
```
class MainApplication : Application() {

    override fun onCreate() {
        super.onCreate()

        startKoin {
            // Koin Android logger
            androidLogger()
            //inject Android context
            androidContext(this@MainApplication)
            // use modules
            modules(myAppModules)
        }
        
    }
}
```

### [Starting Koin with Android context from elsewhere?](https://insert-koin.io/docs/reference/koin-android/start/#starting-koin-with-android-context-from-elsewhere)
If you need to start Koin from another Android class, you can use the `startKoin` function and provide your Android `Context` instance with just like:
```
startKoin {
    //inject Android context
    androidContext(/* your android context */)
    // use modules
    modules(myAppModules)
}
```

## [Injecting in Android](https://insert-koin.io/docs/reference/koin-android/get-instances/)

### [Activity, Fragment & Service as KoinComponents](https://insert-koin.io/docs/reference/koin-android/get-instances#activity-fragment--service-as-koincomponents)
`Activity`, `Fragment` & `Service` are extended with the `KoinComponents` extension. You gain access to:
- `by inject()` - lazy evaluated instance from Koin container;
- `get()` - eager fetch instance from Koin container.

For a module that declares a 'presenter' component:
```
val androidModule = module {
    // a factory of Presenter
    factory { Presenter() }
}
```

We can declare a property as lazy injected:
```
class DetailActivity : AppCompatActivity() {

    // Lazy injected Presenter instance
    override val presenter : Presenter by inject()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // ...
    }
```

Or we can just directly get an instance:
```
class DetailActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Retrieve a Presenter instance
        val presenter : Presenter = get()
    }
```

## [Injecting Android ViewModel](https://insert-koin.io/docs/reference/koin-android/viewmodel/)
The `koin-android` Gradle module introduces a new `viewModel` DSL keyword that comes in complement of `single` and `factory`, to help declare a ViewModel component and bind it to an Android Component lifecycle.
```
val appModule = module {

    // ViewModel for Detail View
    viewModel { DetailViewModel(get(), get()) }

}
```
Your declared component must at least extends the `android.arch.lifecycle.ViewModel` class. You can specify how you inject the *constructor* of the class and use the `get()` function to inject dependencies.

**Info**
- The `viewModel` keyword helps declaring a factory instance of ViewModel. This instance will be handled by internal `ViewModelFactory` and reattach ViewModel instance if needed;
- The `viewModel` keyword can also let you use the injection parameters.

### [Injecting your ViewModel](https://insert-koin.io/docs/reference/koin-android/viewmodel/#injecting-your-viewmodel)
To inject a ViewModel in an `Activity`, `Fragment` or `Service` use:
- `by viewModel()` - lazy delegate property to inject a ViewModel into a property;
- `getViewModel()` - directly get the ViewModel instance.

```
class DetailActivity : AppCompatActivity() {

    // Lazy inject ViewModel
    val detailViewModel: DetailViewModel by viewModel()
}
```

# Links
[koin](https://github.com/InsertKoinIO/koin)

[Koin documentation](https://insert-koin.io/docs/reference/introduction)

# Further reading
[Kotzilla - Latest News](https://blog.kotzilla.io/)
