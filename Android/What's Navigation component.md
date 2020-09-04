# Navigation component
The [Navigation Architecture Component](https://developer.android.com/guide/navigation) simplifies implementing navigation, while also helping you visualize your app's navigation flow. The library provides a number of benefits, including:
- Automatic handling of fragment transactions
- Correctly handling *up* and *back* by default
- Default behaviors for animations and transitions
- Deep linking as a first class operation
- Implementing navigation UI patterns (like navigation drawers and bottom nav) with little additional work
- Type safety when passing information while navigating
- Android Studio tooling for visualizing and editing the navigation flow of an app

The Navigation component consists of three key parts that are described below:
- Navigation graph: An XML resource that contains all navigation-related information in one centralized location. This includes all of the individual content areas within your app, called *destinations*, as well as the possible paths that a user can take through your app.
- `NavHost`: An empty container that displays destinations from your navigation graph. The Navigation component contains a default `NavHost` implementation, `NavHostFragment`, that displays fragment destinations.
- `NavController`: An object that manages app navigation within a `NavHost`. The `NavController` orchestrates the swapping of destination content in the `NavHost` as users move throughout your app.
As you navigate through your app, you tell the `NavController` that you want to navigate either along a specific path in your navigation graph or directly to a specific destination. The `NavController` then shows the appropriate destination in the `NavHost`.

To include Navigation support in your project, add the following dependencies to your app's `build.gradle` file:
```
dependencies {
  def nav_version = "2.3.0"

  // Java language implementation
  implementation "androidx.navigation:navigation-fragment:$nav_version"
  implementation "androidx.navigation:navigation-ui:$nav_version"

  // Kotlin
  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

  // Feature module Support
  implementation "androidx.navigation:navigation-dynamic-features-fragment:$nav_version"

  // Testing Navigation
  androidTestImplementation "androidx.navigation:navigation-testing:$nav_version"
}
```

**navigation-runtime**: This core library powers the navigation graph, which provides the structure of your in-app navigation: the screens or destinations that make up your app and the actions that link them. You can control how you navigate to destinations with a simple `navigate()` call. These destinations may be fragments, activities or custom destinations.

**navigation-fragment**: This library builds upon navigation-runtime and provides out-of-the-box support for fragments as destinations. With this library, fragment transactions are now handled for you automatically.

**navigation-ui**: This library allows you to easily add navigation drawers, menus and bottom navigation to your app consistent with the Material Design guidelines.

## Links
https://developer.android.com/guide/navigation  
https://developer.android.com/guide/navigation/navigation-getting-started  
https://codelabs.developers.google.com/codelabs/android-navigation
