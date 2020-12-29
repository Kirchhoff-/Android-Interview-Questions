# Jetpack
Jetpack encompasses a collection of Android libraries that incorporate best practices and provide backwards compatibility in your Android apps. The Android Jetpack components bring together the existing Support Library and Architecture Components and arranges them into four categories:

![](./res/android_jetpack.png "Android jetpack")
[Android Jetpack](https://android-developers.googleblog.com/2018/05/use-android-jetpack-to-accelerate-your.html)

Android Jetpack components are provided as "unbundled" libraries that are not part of the underlying Android platform. This means that you can adopt each component at your own speed, at your own time. When new Android Jetpack functionality is available, you can add it to your app.

In addition, your app can run on various versions of the platform because Android Jetpack components are built to provide their functionality independent of any specific version, providing backwards compatibility.

Further, Android Jetpack is built around modern design practices like separation of concerns and testability as well as productivity features like Kotlin integration. This makes it far easier for you to build robust, high quality apps with less code. While the components of Android Jetpack are built to work together, e.g. lifecycle awareness and live data, you don't have to use all of them -- you can integrate the parts of Android Jetpack that solve your problems while keeping the parts of your app that are already working great.

## [Use a Jetpack library in your app](https://developer.android.com/jetpack/getting-started#use_a_jetpack_library_in_your_app)

All Jetpack components are available on the [Google Maven repository](https://maven.google.com/web/index.html).

Open the `build.gradle` file for your project and add the `google()` repository as shown below:
```
allprojects {
    repositories {
        google()
        jcenter()
    }
}
```

You can then add Jetpack components, such as architecture components like `LiveData` and `ViewModel`, as shown here:
```
dependencies {
    def lifecycle_version = "2.2.0"
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$lifecycle_version"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    ...
}
```

Many Jetpack libraries provide [Android KTX extensions](https://developer.android.com/kotlin/ktx) as shown above with `lifecycle-livedata-ktx` and `lifecycle-viewmodel-ktx`. The KTX extensions build upon the Java-based API, taking advantage of Kotlin-specific language features.

## Main libraries

### WorkManager
The `WorkMananager` component is a powerful new library that provides a one-stop solution for constraint-based background jobs that need guaranteed execution, replacing the need to use things like jobs or `SyncAdapters`. `WorkManager` provides a simplified, modern API, the ability to work on devices with or without Google Play Services, the ability to create graphs of work, and the ability to query the state of your work. 

### Navigation
The [Navigation Architecture Component](https://developer.android.com/guide/navigation) simplifies implementing navigation, while also helping you visualize your app's navigation flow. The library provides a number of benefits, including:

- Automatic handling of fragment transactions
- Correctly handling up and back by default
- Default behaviors for animations and transitions
- Deep linking as a first class operation
- Implementing navigation UI patterns (like navigation drawers and bottom nav) with little additional work
- Type safety when passing information while navigating
- Android Studio tooling for visualizing and editing the navigation flow of an app

### Paging
The Paging Library helps you load and display small chunks of data at a time. Loading partial data on demand reduces usage of network bandwidth and system resources.

The Paging Library supports the following data architectures:
- Served only from a backend server;
- Stored only in an on-device database;
- A combination of the other sources, using the on-device database as a cache.

### Room
The Room persistence library provides an abstraction layer over SQLite to allow for more robust database access while harnessing the full power of SQLite.

The library helps you create a cache of your app's data on a device that's running your app. This cache, which serves as your app's single source of truth, allows users to view a consistent copy of key information within your app, regardless of whether users have an internet connection.

### ViewModel
The `ViewModel` class is designed to store and manage UI-related data in a lifecycle conscious way. The `ViewModel` class allows data to survive configuration changes such as screen rotations.

The Android framework manages the lifecycles of UI controllers, such as activities and fragments. The framework may decide to destroy or re-create a UI controller in response to certain user actions or device events that are completely out of your control.

### Lifecycle
Lifecycle-aware components perform actions in response to a change in the lifecycle status of another component, such as activities and fragments. These components help you produce better-organized, and often lighter-weight code, that is easier to maintain.

A common pattern is to implement the actions of the dependent components in the lifecycle methods of activities and fragments. However, this pattern leads to a poor organization of the code and to the proliferation of errors. By using lifecycle-aware components, you can move the code of dependent components out of the lifecycle methods and into the components themselves.

The `androidx.lifecycle` package provides classes and interfaces that let you build lifecycle-aware components—which are components that can automatically adjust their behavior based on the current lifecycle state of an activity or fragment.

# Links
https://developer.android.com/jetpack/getting-started  
https://android-developers.googleblog.com/2018/05/use-android-jetpack-to-accelerate-your.html  
https://medium.com/androiddevelopers/whats-new-in-jetpack-1891d205e136

# Next questions
[What's WorkManager](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20WorkManager.md)  
[What's Navigation component](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20Navigation%20component.md)  
[What you know about paging library](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20you%20know%20about%20paging%20library.md)  
[What's Room](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20Room.md)  
[What's ViewModel](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20ViewModel.md)  
[What you know about lifecycle from architecture component](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20you%20know%20about%20lifecycle%20from%20architecture%20component.md)
