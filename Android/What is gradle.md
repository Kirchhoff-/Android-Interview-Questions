# Gradle
Gradle is an open-source build automation tool flexible enough to build almost any type of software. Gradle makes few assumptions about what you’re trying to build or how to build it. This makes Gradle particularly flexible.<sup>[1](https://docs.gradle.org/current/userguide/what_is_gradle.html#:~:text=Gradle%20is%20an%20open%2Dsource%20build%20automation%20tool%20flexible%20enough%20to%20build%20almost%20any%20type%20of%20software.%20Gradle%20makes%20few%20assumptions%20about%20what%20you%E2%80%99re%20trying%20to%20build%20or%20how%20to%20build%20it.%20This%20makes%20Gradle%20particularly%20flexible.)</sup>

Android Studio uses Gradle to automate and manage the build process while letting you define flexible, custom build configurations. Each build configuration can define its own set of code and resources while reusing the parts common to all versions of your app. The Android Gradle plugin works with the build toolkit to provide processes and configurable settings that are specific to building and testing Android apps. 

Gradle and the Android Gradle plugin run independent of Android Studio. This means that you can build your Android apps from within Android Studio, the command line on your machine, or on machines where Android Studio is not installed, such as continuous integration servers. <sup>[2](https://developer.android.com/build#:~:text=Android%20Studio%20uses,continuous%20integration%20servers.)</sup>

Gradle runs on Groovy DSL (domain specific language) and most recently, Kotlin DSL.

## [Android build glossary](https://developer.android.com/build#build-config)
Gradle and the Android Gradle plugin help you configure the following aspects of your build: 

**Build types**. 

Build types define certain properties that Gradle uses when building and packaging your app. Build types are typically configured for different stages of your development lifecycle.

For example, the debug build type enables debug options and signs the app with the debug key, while the release build type may shrink, obfuscate, and sign your app with a release key for distribution.

You must define at least one build type to build your app. Android Studio creates the debug and release build types by default. 

**Product flavors**

Product flavors represent different versions of your app that you can release to users, such as free and paid versions. You can customize product flavors to use different code and resources while sharing and reusing the parts that are common to all versions of your app. Product flavors are optional, and you must create them manually. 

**Build variants**

A build variant is a cross-product of build type and product flavor and is the configuration Gradle uses to build your app. Using build variants, you can build the debug version of your product flavors during development and signed release versions of your product flavors for distribution. Although you don't configure build variants directly, you configure the build types and product flavors that form them. Creating additional build types or product flavors also creates additional build variants. 

**Manifest entries**

You can specify values for some properties of the manifest file in the build variant configuration. These build values override the existing values in the manifest file. This is useful if you want to generate multiple variants of your app with a different application name, minimum SDK version, or target SDK version. When multiple manifests are present, the manifest merger tool merges manifest settings. 

**Dependencies**

The build system manages project dependencies from your local file system and from remote repositories. This means you don't have to manually search, download, and copy binary packages of your dependencies into your project directory.

**Signing**

The build system lets you specify signing settings in the build configuration, and it can automatically sign your app during the build process. The build system signs the debug version with a default key and certificate using known credentials to avoid a password prompt at build time. The build system does not sign the release version unless you explicitly define a signing configuration for this build.

**Code and resource shrinking**

The build system lets you specify a different ProGuard rules file for each build variant. When building your app, the build system applies the appropriate set of rules to shrink your code and resources using its built-in shrinking tools, such as R8. Shrinking your code and resources can help reduce your APK or AAB size. 

**Multiple APK support**

The build system lets you automatically build different APKs that each contain only the code and resources needed for a specific screen density or Application Binary Interface (ABI).

## [Build configuration files](https://developer.android.com/build)
When starting a new project, Android Studio automatically creates gradle files for you and populates them based on sensible defaults. The project file structure has the following layout: 
```
└── MyApp/  # Project
    ├── build.gradle.kts
    ├── settings.gradle
    └── app/  # Module
        ├── build.gradle.kts
        └── build/
            ├── libs/
            └── src/
                └── main/  # Source set
                    ├── java/
                    │   └── com.example.myapp
                    ├── res/
                    │   ├── drawable/
                    │   ├── values/
                    │   └── ...
                    └── AndroidManifest.xml
```
There are a few Gradle build configuration files that are part of the standard project structure for an Android app. 

[**The Gradle settings file**](https://developer.android.com/build#settings-file)
The `settings.gradle.kts` file (for the Kotlin DSL) or `settings.gradle` file (for the Groovy DSL) is located in the root project directory. This settings file defines project-level repository settings and informs Gradle which modules it should include when building your app. Multi-module projects need to specify each module that should go into the final build. 

[**The top-level build file**](https://developer.android.com/build#top-level)
The top-level `build.gradle.kts` file (for the Kotlin DSL) or `build.gradle` file (for the Groovy DSL) is located in the root project directory. It defines dependencies that apply to all modules in your project. By default, the top-level build file uses the `plugins` block to define the Gradle dependencies that are common to all modules in the project. In addition, the top-level build file contains code to clean your build directory.

[**The module-level build file**](https://developer.android.com/build#module-level)
The module-level `build.gradle.kts` (for the Kotlin DSL) or `build.gradle` file (for the Groovy DSL) is located in each `project/module/` directory. It lets you configure build settings for the specific module it is located in. Configuring these build settings lets you provide custom packaging options, such as additional build types and product flavors, and override settings in the `main/` app manifest or top-level build script. 

[**Gradle properties files**](https://developer.android.com/build#properties-files)
Gradle also includes two properties files, located in your root project directory, that you can use to specify settings for the Gradle build toolkit itself: 
`gradle.properties`. This is where you can configure project-wide Gradle settings, such as the Gradle daemon's maximum heap size.

`local.properties`.
 Configures local environment properties for the build system, including the following: 
 - `ndk.dir` - Path to the NDK. This property has been deprecated. Any downloaded versions of the NDK are installed in the `ndk` directory within the Android SDK directory;
 - `sdk.dir` - Path to the SDK;
 - `cmake.dir` - Path to CMake;
 - `ndk.symlinkdir` - In Android Studio 3.5 and higher, creates a symlink to the NDK that can be shorter than the installed NDK path.

# Links
[Configure your build](https://developer.android.com/build)

# Next questions
[Build Type, Product Flavor, Build Variant](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/Build%20Type%2C%20Product%20Flavor%2C%20Build%20Variant.md)

[What's Proguard?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20Proguard.md)

[What do you know about App Bundles?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20App%20Bundles.md)

# Further Reading
[Gradle Basics for Android Developers](https://medium.com/android-dev-corner/gradle-basics-for-android-developers-9d7a3bf062bb)

[Gradle Tutorial for Android: Getting Started](https://www.kodeco.com/249-gradle-tutorial-for-android-getting-started)
