# Dark theme
Dark theme is available in Android 10 (API level 29) and higher. It has many benefits:
- Can reduce power usage by a significant amount (depending on the device’s screen technology).
- Improves visibility for users with low vision and those who are sensitive to bright light.
- Makes it easier for anyone to use a device in a low-light environment.

Dark theme applies to both the Android system UI and apps running on the device.

## Supporting Dark theme in your app

In order to support Dark theme, you must set your app's theme (usually found in `res/values/styles.xml`) to inherit from a `DayNight` theme:

```
<style name="AppTheme" parent="Theme.AppCompat.DayNight">
```

You can also use MaterialComponents' dark theming:
```
<style name="AppTheme" parent="Theme.MaterialComponents.DayNight">
```

This ties the app's main theme to the system-controlled night mode flags and gives the app a default Dark theme (when it is enabled).

## Themes and styles

This ties the app's main theme to the system-controlled night mode flags and gives the app a default Dark theme (when it is enabled).

Here are the two most important theme attributes to know about:
- `?android:attr/textColorPrimary` This is a general purpose text color. It is near-black in Light theme and near-white in Dark themes. It contains a disabled state.
- `?attr/colorControlNormal` A general-purpose icon color. It contains a disabled state.

## Changing themes in-app

You might want to allow users to change the app's theme while the app is running. Your app can let the user choose between themes. The recommended options are:
- Light
- Dark
- System default (*the recommended default option*)

Each of the options map directly to one of the AppCompat.DayNight modes:

- Light - `MODE_NIGHT_NO`
- Dark - `MODE_NIGHT_YES`
- System default - `MODE_NIGHT_FOLLOW_SYSTEM`

## Configuration changes

When the app’s theme changes (either through the system setting or AppCompat) it triggers a `uiMode` configuration change. This means that Activities will be automatically recreated.

In some cases you might want an app to handle the configuration change. For example, you might want to delay a configuration change because a video is playing.

An app can handle the implementation of Dark theme itself by declaring that each Activity can handle the `uiMode` configuration change:

```
<activity
    android:name=".MyActivity"
    android:configChanges="uiMode" />
 ```
 
 When an Activity declares it handles configuration changes, its `onConfigurationChanged()` method will be called when there is a theme change.
 
## Best Practices

### Notifications and Widgets
For UI surfaces that you display on the device but do not directly control, it is important to make sure that any views you use reflect the host app’s theme. Two good examples are notifications and launcher widgets.

### Notifications
Use the system-provided notification templates (such as `MessagingStyle`). This means that the system is responsible for ensuring the correct view styling is applied.

### Widgets and custom notification views
For launcher widgets, or if your app uses custom notification content views, it is important to make sure you test the content on both the Light and Dark themes.

Common pitfalls to look out for:

- Assuming that the background color is always light
- Hardcoding text colors
- Setting a hardcoded background color, while using the default text color
- Using a drawable icon which is a static color

In all of these cases, use appropriate theme attributes instead of hardcoded colors.

### Launch screens

If your app has a custom launch screen, it may need to be modified so that it reflects the selected theme.

Remove any hardcoded colors, for example any background colors pointing may be white. Use the `?android:attr/colorBackground` theme attribute instead.

Note that dark-themed `android:windowBackground` drawables only work on Android Q.

## Links
https://developer.android.com/guide/topics/ui/look-and-feel/darktheme
