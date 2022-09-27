# Dependency injection libraries

[Dagger 2](https://github.com/google/dagger) - A fast dependency injector for Java and Android. Dagger is a compile-time framework for dependency injection. It uses no reflection or runtime bytecode generation, does all its analysis at compile-time, and generates plain Java source code.

One of the primary advantages of Dagger 2 over most other dependency injection frameworks is that its strictly generated implementation (no reflection) means that it can be used in Android applications.

[Hilt](https://developer.android.com/training/dependency-injection/hilt-android) - Hilt is a dependency injection library for Android that reduces the boilerplate of doing manual dependency injection in your project. 

Hilt provides a standard way to use DI in your application by providing containers for every Android class in your project and managing their lifecycles automatically. Hilt is built on top of the popular DI library Dagger to benefit from the compile-time correctness, runtime performance, scalability, and Android Studio support that Dagger provides.

[Koin](https://github.com/InsertKoinIO/koin) - A pragmatic lightweight dependency injection framework for Kotlin developers. Written in pure Kotlin, using functional resolution only: no proxy, no code generation, no reflection. Koin is a DSL, a lightweight container and a pragmatic API.

[Kodein](https://github.com/Kodein-Framework/Kodein-DI) - is a very simple and yet very useful dependency retrieval container. it is very easy to use and configure.

Kodein-DI allows you to:

- Lazily instantiate your dependencies when needed
- Stop caring about dependency initialization order
- Easily bind classes or interfaces to their instance or provider
- Easily debug your dependency bindings and recursions

Kodein-DI is a good choice because:

- It proposes a very simple and readable declarative DSL
- It is not subject to type erasure (as Java is)
- It integrates nicely with Android
- It proposes a very kotlin-esque idiomatic API
- It is fast and optimized (makes extensive use of inline)
- It can be used in plain Java

# Links
[dagger](https://github.com/google/dagger)

[Dependency injection with Hilt](https://developer.android.com/training/dependency-injection/hilt-android)

[koin](https://github.com/InsertKoinIO/koin)

[Kodein](https://github.com/kosi-libs/Kodein)

# Next questions 
[What's Dagger 2?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/di_libraries_links_fix/Libraries/What's%20Dagger%202.md)

[What do you know about hilt?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/di_libraries_links_fix/Libraries/What%20do%20you%20know%20about%20hilt.md)

[What do you know about Koin?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/di_libraries_links_fix/Libraries/What%20do%20you%20know%20about%20Koin.md)
