# Version catalog
[Gradle version catalogs](https://docs.gradle.org/current/userguide/platforms.html) enable you to add and maintain dependencies and plugins in a scalable way. Using Gradle version catalogs makes managing dependencies and plugins easier when you have multiple modules. Instead of hardcoding dependency names and versions in individual build files and updating each entry whenever you need to upgrade a dependency, you can create a central *version catalog* of dependencies that various modules can reference in a type-safe way with Android Studio assistance.<sup>[1](https://developer.android.com/build/migrate-to-catalogs#:~:text=Gradle%20version%20catalogs,Android%20Studio%20assistance.)</sup>

A version catalog provides a number of advantages over declaring the dependencies directly in build scripts<sup>[2](
https://docs.gradle.org/current/userguide/platforms.html#:~:text=In%20this%20context,between%20multiple%20dependencies.)</sup>:
- For each catalog, Gradle generates *type-safe accessors* so that you can easily add dependencies with autocompletion in the IDE;
- Each catalog is visible to all projects of a build. It is a central place to declare a version of a dependency and to make sure that a change to that version applies to every subproject;
- Catalogs can declare [dependency bundles](), which are "groups of dependencies" that are commonly used together;
- Catalogs can separate the group and name of a dependency from its actual version and use [version references](https://docs.gradle.org/current/userguide/platforms.html#sec:common-version-numbers) instead, making it possible to share a version declaration between multiple dependencies.

## [Create a version catalog file](https://developer.android.com/build/migrate-to-catalogs#create-version)
Start by creating a version catalog file. In your root project's `gradle` folder, create a file called `libs.versions.toml`. Gradle looks for the catalog in the `libs.versions.toml` file by [default](https://docs.gradle.org/current/userguide/platforms.html#sub:conventional-dependencies-toml), so we recommend using this default name.

In your `libs.versions.toml` file, add these sections:
```
[versions]

[libraries]

[plugins]

[bundles]
```

## [The version catalog TOML file format](https://docs.gradle.org/current/userguide/platforms.html#sub::toml-dependencies-format)
The TOML file consists of 4 major sections:
- the `[versions]` section is used to declare versions which can be referenced by dependencies;
- the `[libraries]` section is used to declare the aliases to coordinates;
- the `[bundles]` section is used to declare dependency bundles;
- the `[plugins]` section is used to declare plugins.

For example:
```
[versions]
groovy = "3.0.5"
checkstyle = "8.37"

[libraries]
groovy-core = { module = "org.codehaus.groovy:groovy", version.ref = "groovy" }
groovy-json = { module = "org.codehaus.groovy:groovy-json", version.ref = "groovy" }
groovy-nio = { module = "org.codehaus.groovy:groovy-nio", version.ref = "groovy" }

[bundles]
groovy = ["groovy-core", "groovy-json", "groovy-nio"]

[plugins]
versions = { id = "com.github.ben-manes.versions", version = "0.45.0" }
```

### [Aliases and their mapping to type safe accessors](https://docs.gradle.org/current/userguide/platforms.html#sub:mapping-aliases-to-accessors)
Aliases must consist of a series of identifiers separated by a dash (`-`, recommended), an underscore (`_`) or a dot (`.`). Identifiers themselves must consist of ascii characters, preferably lowercase, eventually followed by numbers.

For example:
- `guava` is a valid alias;
- `groovy-core` is a valid alias;
- `commons-lang3` is a valid alias;
- `androidx.awesome.lib` is also a valid alias;
- but `this.#is.not`!

Then type safe accessors are generated for *each subgroup*. For example, given the following aliases in a version catalog named `libs`:
```
guava, groovy-core, groovy-xml, groovy-json, androidx.awesome.lib
```

We would generate the following type-safe accessors:
- `libs.guava`;
- `libs.groovy.core`;
- `libs.groovy.xml`;
- `libs.groovy.json`;
- `libs.androidx.awesome.lib`.

Where the `libs` prefix comes from the version catalog name.

## [Dependency bundles](https://docs.gradle.org/current/userguide/platforms.html#sec:dependency-bundles)
Because it’s frequent that some dependencies are systematically used together in different projects, a version catalog offers the concept of a "dependency bundle". A bundle is basically an alias for several dependencies. For example, suppose you have such dependencies: 
```
dependencies {
    implementation libs.groovy.core
    implementation libs.groovy.json
    implementation libs.groovy.nio
}
```

instead of declaring 3 individual dependencies like above, you could write:
```
dependencies {
    implementation libs.bundles.groovy
}
```

The bundle named groovy needs to be declared in the catalog:
```
dependencyResolutionManagement {
    versionCatalogs {
        libs {
            version('groovy', '3.0.5')
            version('checkstyle', '8.37')
            library('groovy-core', 'org.codehaus.groovy', 'groovy').versionRef('groovy')
            library('groovy-json', 'org.codehaus.groovy', 'groovy-json').versionRef('groovy')
            library('groovy-nio', 'org.codehaus.groovy', 'groovy-nio').versionRef('groovy')
            bundle('groovy', ['groovy-core', 'groovy-json', 'groovy-nio'])
        }
    }
}
```

The semantics are again equivalent: adding a single bundle is equivalent to adding all dependencies which are part of the bundle individually.

## [Plugins](https://docs.gradle.org/current/userguide/platforms.html#sec:plugins)
In addition to libraries, version catalog supports declaring plugin versions. While libraries are represented by their group, artifact and version coordinates, Gradle plugins are identified by their id and version only. Therefore, they need to be declared separately:
```
dependencyResolutionManagement {
    versionCatalogs {
        libs {
            plugin('versions', 'com.github.ben-manes.versions').version('0.45.0')
        }
    }
}
```

Then the plugin is accessible in the `plugins` block and can be consumed in any project of the build using:
```
plugins {
    id 'java-library'
    id 'checkstyle'
    // Use the plugin `versions` as declared in the `libs` version catalog
    alias(libs.plugins.versions)
}
```

# Links
[Migrate your build to version catalogs](https://developer.android.com/build/migrate-to-catalogs)

[Sharing dependency versions between projects](https://docs.gradle.org/current/userguide/platforms.html)

# Further Reading
[Using Version Catalog on Android projects](https://proandroiddev.com/using-version-catalog-on-android-projects-82d88d2f79e5)

[Is the New Gradle Version Catalog Worth It for Your Android Projects?](https://molidevwrites.com/is-the-new-gradle-version-catalog-worth-it-for-your-android-projects/)
