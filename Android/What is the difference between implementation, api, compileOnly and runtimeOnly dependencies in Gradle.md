# `implementation`, `api`, `compileOnly` and `runtimeOnly` dependencies in Gradle
The Gradle build system in Android Studio lets you include external binaries or other library modules to your build as dependencies. The dependencies can be located on your machine or in a remote repository, and any transitive dependencies they declare are automatically included as well.<sup>[1](https://developer.android.com/build/dependencies?hl=en#:~:text=The%20Gradle%20build%20system%20in%20Android%20Studio%20lets%20you%20include%20external%20binaries%20or%20other%20library%20modules%20to%20your%20build%20as%20dependencies.%20The%20dependencies%20can%20be%20located%20on%20your%20machine%20or%20in%20a%20remote%20repository%2C%20and%20any%20transitive%20dependencies%20they%20declare%20are%20automatically%20included%20as%20well.)</sup>

Inside the dependencies block, you can declare a library dependency using one of several different *dependency configurations*. Each dependency configuration provides Gradle with different instructions about how to use the dependency:<sup>[2](https://developer.android.com/build/dependencies?hl=en#dependency_configurations)</sup>
- `api`. Gradle adds the dependency to the compile classpath and build output. When a module includes an `api` dependency, it's letting Gradle know that the module wants to transitively export that dependency to other modules, so that it's available to them at both runtime and compile time. 

  Use this configuration with caution and only with dependencies that you need to transitively export to other upstream consumers. If an `api` dependency changes its external API, Gradle recompiles all modules that have access to that dependency at compile time. Having a large number of api dependencies can significantly increase build time. Unless you want to expose a dependency's API to a separate module, library modules should instead use `implementation` dependencies;
- `implementation`. Gradle adds the dependency to the compile classpath and packages the dependency to the build output. When your module configures an `implementation` dependency, it's letting Gradle know that you don't want the module to leak the dependency to other modules at compile time. That is, the dependency isn't made available to other modules that depend on the current module. 

  Using this dependency configuration instead of `api` can result in significant build time improvements because it reduces the number of modules that the build system needs to recompile. For example, if an `implementation` dependency changes its API, Gradle recompiles only that dependency and the modules that directly depend on it. Most app and test modules should use this configuration;
- `compileOnly`. Gradle adds the dependency to the compile classpath only (that is, it's not added to the build output). This is useful when you're creating an Android module and you need the dependency during compilation, but it's optional to have it present at runtime. For example, if you depend on a library that only includes compile-time annotations—typically used to generate code but often not included in the build output—you could mark that library `compileOnly`. 

  If you use this configuration, then your library module must include a runtime condition to check whether the dependency is available, and then gracefully change its behavior so it can still function if it's not provided. This helps reduce the size of the final app by not adding transient dependencies that aren't critical.

- `runtimeOnly`. Gradle adds the dependency to the build output only, for use during runtime. That is, it isn't added to the compile classpath. This is rarely used on Android, but commonly used in server applications to provide logging implementations. For example, a library could use a logging API that doesn't include an implementation. Consumers of that library could add it as an `implementation` dependency and include a `runtimeOnly` dependency for the actual logging implementation to use;

## [Example to Demonstrate Difference](https://codemia.io/knowledge-hub/path/whats_the_difference_between_implementation_api_and_compile_in_gradle#:~:text=Example%20to%20Demonstrate%20Difference)
To cement these concepts, consider a project structured as follows:
- `App Module`: Depends on `Library Module A`;
- `Library Module A`: Uses a logging library.

If `Library Module A` defines its logging library dependency using `api`, the `App Module` will also have access to the logging library without explicitly declaring it in its dependencies. However, if `Library Module A` uses `implementation` to declare its logging library, then the `App Module` won't have access to the logging library unless it explicitly declares it too.
```
// In Library Module A's build.gradle
dependencies {
    api 'com.example.logging:logging-library:1.0.0' // Accessible to App Module
    // vs
    implementation 'com.example.logging:logging-library:1.0.0' // Not accessible to App Module
}
```


# Links
[Add build dependencies](https://developer.android.com/build/dependencies)

[What's the difference between implementation, api and compile in Gradle?](https://codemia.io/knowledge-hub/path/whats_the_difference_between_implementation_api_and_compile_in_gradle)

# Further reading
[What's the difference between "implementation", "api" and "compile" in Gradle?](https://stackoverflow.com/questions/44493378/whats-the-difference-between-implementation-api-and-compile-in-gradle)

[Difference Between implementation and compile in Gradle](https://www.baeldung.com/gradle-implementation-vs-compile)
