# Main annotations in Dagger 2
The main annotations in Dagger 2 are: 
- `@Provides`
- `@Inject`
- `@Module`
- `@Component`
- `@Singleton`

## Provides
This annotation used for method that describe how to create instance of the class

```
@Provides
fun provideSomeClass(): SomeClass = SomeClass()
```

## Singleton
Annotate an `@Provides` method or injectable class with `@Singleton`. The graph will use a single instance of the value for all of its clients.

```
@Provides
@Singleton
fun provideSomeClass(): SomeClass = SomeClass()
```

## Inject
the @Inject annotation will tell the Dagger what all the dependencies needed to be transferred to the dependent. Can be used in constructor, field or method.

Constructor injection:
```
class SomeClass @Inject constructor(
      private val interactor: Interactor,
      private val useCase: UseCase
) {...}
```

Field injection:
```
class SomeClass() {
  @Inject lateinit var viewModel: SomeViewModel
} {...}
```

Method injection:
```
class SomeClass() {
  lateinit var user: User

  @Inject
  fun getUserName(userFromPreference: UserFromPreference) {
    user = userFromPreference.getUser()
  }
}
```

Dagger 2 manages the dependency graph of the classes that have the `@Inject` and `@Provides` annotation. Dagger cannot instantiate or inject classes that do not have neither `@Inject` nor `@Provides` annotations.

## Module
All `@Provides` methods must belong to a module. These are just classes that have an `@Module` annotation. Module provides the instance of dependencies.

```
@Module
interface SomeModule {
  @Provides
  @Singleton
  fun provideSomeClass(): SomeClass = SomeClass()
}
```

## Component
The `@Component` is used on an interface. Such an interface is used by Dagger 2 to generate code. A `@Component` interface defines the connection between provider of objects (modules) and the objects which expresses a dependency. 

```
@Component(modules = SomeModule.class)
interface ModuleComponent {
  SomeClass someClassInstance();
}
```

# Links
[Using Dagger 2 for dependency injection in Android - Tutorial](https://www.vogella.com/tutorials/Dagger/article.html)

[Dagger](https://dagger.dev/dev-guide/)

[Dependency injection with Dagger 2: @Inject and @Provides](https://medium.com/@yostane/dependency-injection-with-dagger-2-inject-and-provides-ce21f7449ec5)

[Dagger 2- Annotations](https://blog.canopas.com/dagger-2-annotation-b3a27d53dabf)
