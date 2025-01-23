# `CompositionLocal`
Usually in Compose, data flows down through the UI tree as parameters to each composable function. This makes a composable’s dependencies explicit. This can however be cumbersome for data that is very frequently and widely used such as colors or type styles. See the following example:
```
@Composable
fun MyApp() {
    // Theme information tends to be defined near the root of the application
    val colors = colors()
}

// Some composable deep in the hierarchy
@Composable
fun SomeTextLabel(labelText: String) {
    Text(
        text = labelText,
        color = colors.onPrimary // ← need to access colors here
    )
}
```

To support not needing to pass the colors as an explicit parameter dependency to most composables, **Compose offers** `CompositionLocal` **which allows you to create tree-scoped named objects that can be used as an implicit way to have data flow through the UI tree**.

`CompositionLocal` elements are usually provided with a value in a certain node of the UI tree. That value can be used by its composable descendants without declaring the `CompositionLocal` as a parameter in the composable function.<sup>[1](https://developer.android.com/develop/ui/compose/compositionlocal?hl=en#:~:text=Usually%20in%20Compose,the%20composable%20function.)</sup>

Example of usage of `CompositionLocal`:
```
data class UserPreferences(val isDarkModeEnabled: Boolean, val fontSize: Float)

// Define a dynamic Composition Local with default preferences
val LocalUserPreferences = compositionLocalOf {
    UserPreferences(isDarkModeEnabled = false, fontSize = 14f)
}
@Composable
fun PreferencesProvider(content: @Composable () -> Unit) {
    var isDarkMode by remember { mutableStateOf(false) }
    val preferences = UserPreferences(isDarkMode, 16f)
    CompositionLocalProvider(LocalUserPreferences provides preferences) {
        content()
    }
}
@Composable
fun SettingsScreen() {
    val preferences = LocalUserPreferences.current
    Text("Dark Mode: ${if (preferences.isDarkModeEnabled) "Enabled" else "Disabled"}")
}
```

Explanation:
- **Definition**. The `UserPreferences` data class holds dynamic user settings;
- **Provision**. The `PreferencesProvider` dynamically updates and provides the preferences based on the user's actions;
- **Consumption**. The `SettingsScreen` consumes and displays the preferences, updating reactively when changes occur.<sup>[2](https://medium.com/@ramadan123sayed/composition-local-in-jetpack-compose-4d0a54afa67c#:~:text=data%20class%20AppConfig,explicitly%20as%20parameters.)</sup>

## [Key Concepts and Benefits of `CompositionLocal`](https://medium.com/@ramadan123sayed/composition-local-in-jetpack-compose-4d0a54afa67c#:~:text=Key%20Concepts%20and%20Benefits%20of%20CompositionLocal)
- **Implicit Sharing**. `CompositionLocal` enables data to be shared across composables without explicitly passing it as a parameter. This allows easier management of globally relevant data, such as theme styles, user settings, or app-wide configurations;
- **Localized Context**. `CompositionLocal` allows for different parts of the UI to have their own context, which can vary depending on the scope in which the `CompositionLocal` is provided. This is useful for customizing different sections of the UI while maintaining overall consistency;
- **Reactivity**. For dynamic values, `CompositionLocal` can trigger recompositions when its value changes. This means that when a value is updated, only the parts of the UI that depend on this value will be recomposed, keeping the UI efficient and responsive;
- **Decoupled Components**. `CompositionLocal` decouples UI components from their direct dependencies. This enhances the modularity and reusability of composables, as they don’t need to be aware of the specific data provided higher in the composition tree. This makes testing and reusing components easier.

## [Why Are Composition Locals Important?](https://proandroiddev.com/composition-locals-in-jetpack-compose-a-beginner-to-advanced-guide-e6a812ca7620#:~:text=Why%20Are%20Composition%20Locals%20Important%3F)
- **Reduced Boilerplate**. Provide data once at a higher level, consume it wherever necessary;
- **Better Encapsulation**. Intermediate composables don’t need to manage data they don’t directly use;
- **Easier Testing**. You can override Composition Locals in tests without changing public APIs;
- **Scoped Values**. Data is only available to composables within the specific composition scope.

## [Types of CompositionLocal Providers](https://medium.com/@ramadan123sayed/composition-local-in-jetpack-compose-4d0a54afa67c#:~:text=Types%20of%20CompositionLocal%20Providers)
Jetpack Compose offers two APIs to create a `CompositionLocal`, each suited to different scenarios:

`compositionLocalOf`:
- **Fine-grained Control**. This API allows fine control over recompositions. When the value changes, only the parts of the UI that read this value are recomposed. This makes it ideal for frequently changing data like dynamic themes or user preferences;
- **Use Case**. Situations where data changes regularly, such as dynamic UI themes, localization settings, or user-specific configurations.

`staticCompositionLocalOf`:
- **Static Data Handling**. In contrast to `compositionLocalOf`, Compose does not track the places where `staticCompositionLocalOf` is read. When the value changes, the entire content block where the `CompositionLocal` is provided gets recomposed, instead of just the places where the `current` value is read in the Composition. It is best used for data that rarely changes;
- **Use Case**. Suitable for stable configurations like API endpoints, debug flags, or static UI themes that remain constant during the app lifecycle.

If the value provided to the `CompositionLocal` is highly unlikely to change or will never change, use `staticCompositionLocalOf` to get performance benefits.

## [What Composition Locals Are (and Aren’t)](https://proandroiddev.com/composition-locals-in-jetpack-compose-a-beginner-to-advanced-guide-e6a812ca7620#:~:text=What%20Composition%20Locals%20Are%20(and%20Aren%E2%80%99t))
**Are For**:
- Design system tokens (themes, spacing, typography);
- UI infrastructure (keyboard controller, focus management);
- Cross-cutting UI concerns that follow the visual hierarchy;
- Values that need different overrides at different levels of your UI tree.

**Aren’t For**:
- Business logic or application state;
- Data that should be managed by state management solutions;
- Values that affect application behavior — Database connections, repositories, or other non-UI dependencies;
- Avoiding proper dependency injection.

Think of Composition Locals as part of your UI toolkit, not as a general dependency injection mechanism. They shine when dealing with UI-related values that naturally follow your component hierarchy, but shouldn’t be used to bypass proper architecture patterns or hide important dependencies.<sup>[3](https://proandroiddev.com/composition-locals-in-jetpack-compose-a-beginner-to-advanced-guide-e6a812ca7620#:~:text=Are%20For%3A,hide%20important%20dependencies.)</sup>

# Links
[Locally scoped data with CompositionLocal](https://developer.android.com/develop/ui/compose/compositionlocal)

[Composition Local in Jetpack Compose](https://medium.com/@ramadan123sayed/composition-local-in-jetpack-compose-4d0a54afa67c)

[Composition Locals in Jetpack Compose: A Beginner-to-Advanced Guide](https://proandroiddev.com/composition-locals-in-jetpack-compose-a-beginner-to-advanced-guide-e6a812ca7620)

# Further reading
[Jetpack Compose CompositionLocal - What You Need to Know](https://medium.com/geekculture/jetpack-compose-compositionlocal-what-you-need-to-know-979a4aef6412)

[A Jetpack Compose Composition Local Tutorial](https://www.answertopia.com/jetpack-compose/a-jetpack-compose-composition-local-tutorial/)
