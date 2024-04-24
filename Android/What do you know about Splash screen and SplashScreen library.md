# Splash screen and SplashScreen library
The Splash Screen is the first screen visible to the user when the application is launched. This is an important screen, the user gets the first impression of the application on it.

When a user launches an app while the app's process isn't running (a cold start) or the `Activity` isn't created (a warm start), the following events occur:
- The system shows the splash screen using themes and any animations that you define;
- When the app is ready, the splash screen is dismissed and the app displays.

The splash screen never shows during a hot start.<sup>[1](https://developer.android.com/develop/ui/views/launch/splash-screen#how)</sup>

Starting in Android 12, the system applies the [Android system default splash screen](https://developer.android.com/about/versions/12/features/splash-screen) on cold and warm starts for all apps. By default, this system splash screen is constructed using your app's launcher icon element and the `windowBackground` of your theme, if it's a single color.<sup>[2](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate#:~:text=Starting%20in%20Android%2012%2C%20the%20system%20applies%20the%20Android%20system%20default%20splash%20screen%20on%20cold%20and%20warm%20starts%20for%20all%20apps.%20By%20default%2C%20this%20system%20splash%20screen%20is%20constructed%20using%20your%20app%27s%20launcher%20icon%20element%20and%20the%20windowBackground%20of%20your%20theme%2C%20if%20it%27s%20a%20single%20color.)</sup>

## [SplashScreen compat library](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate#library)
You can use the `SplashScreen` API directly, but we strongly recommend using the [Androidx SplashScreen compat library](https://developer.android.com/reference/kotlin/androidx/core/splashscreen/SplashScreen) instead. The compat library uses the `SplashScreen` API, enables backward-compatibility, and creates a consistent look and feel for splash screen display across all Android versions.

If you migrate using the `SplashScreen` API directly, on Android 11 and earlier your splash screen looks exactly the same as before the migration. Starting on Android 12, the splash screen has the Android 12 look and feel.

If you migrate using the `SplashScreen` compat library, the system displays the same splash screen on all versions of Android.

## [Add a splash screen](https://developer.android.com/training/wearables/apps/splash-screen)
To integrate SplashScreen library into your project you should do the next:
- [Add dependencies](#Add-dependencies);
- [Add a theme](#Add-a-theme);
- [Create a drawable for the theme](#Create-a-drawable-for-the-theme);
- [Specify the theme](#Specify-the-theme);
- [Update your starting activity](#Update-your-starting-activity).

### [Add dependencies](https://developer.android.com/training/wearables/apps/splash-screen#add-dependencies)
Add the following dependency to your app module's `build.gradle` file:
```
dependencies {
    implementation("androidx.core:core-splashscreen:1.2.0-alpha01")
}
```

### [Add a theme](https://developer.android.com/training/wearables/apps/splash-screen#add-theme)
Create a splash screen theme in `res/values/styles.xml`. The parent element depends on the icon's shape:
- If the icon is round, use `Theme.SplashScreen`;
- If the icon is a different shape, use `Theme.SplashScreen.IconBackground`.

Use `windowSplashScreenBackground` to fill the background with a single black color. Set the values of `postSplashScreenTheme` to the theme that the Activity should use and `windowSplashScreenAnimatedIcon` to a drawable or animated drawable:
```
<resources>
    <style name="Theme.App" parent="@android:style/Theme.DeviceDefault" />

    <style name="Theme.App.Starting" parent="Theme.SplashScreen">
        <!-- Set the splash screen background to black -->
        <item name="windowSplashScreenBackground">@android:color/black</item>
        <!-- Use windowSplashScreenAnimatedIcon to add a drawable or an animated
             drawable. -->
        <item name="windowSplashScreenAnimatedIcon">@drawable/splash_screen</item>
        <!-- Set the theme of the Activity that follows your splash screen. -->
        <item name="postSplashScreenTheme">@style/Theme.App</item>
    </style>
</resources>
```

If you use a non-round icon, you need to set a white background color underneath your icon. In this case, use the `Theme.SplashScreen.IconBackground` as parent theme and set the `windowSplashScreenIconBackgroundColor` attribute:
```
<style name="Theme.App.Starting" parent="Theme.SplashScreen.IconBackground">
    ...
    <!-- Set a white background behind the splash screen icon. -->
    <item name="windowSplashScreenIconBackgroundColor">@android:color/white</item>
</style>
```

The other attributes are optional.

### [Create a drawable for the theme](https://developer.android.com/training/wearables/apps/splash-screen#create-drawable)
Splash screen themes requires a drawable to pass into the `windowSplashScreenAnimatedIcon` attribute. For example, you can create it by adding a new file `res/drawable/splash_screen.xml` and using app launcher icon and correct splash screen icon size:
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:width="@dimen/splash_screen_icon_size"
        android:height="@dimen/splash_screen_icon_size"
        android:drawable="@mipmap/ic_launcher"
        android:gravity="center" />
</layer-list>
```

The splash screen icon size is defined in `res/values/dimens.xml` and differs depending whether the icon is round:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- Round app icon can take all of default space -->
    <dimen name="splash_screen_icon_size">48dp</dimen>
</resources>
```

...or non-round and therefore must use the icon background:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- Non-round icon with background must use reduced size to fit circle -->
    <dimen name="splash_screen_icon_size">36dp</dimen>
</resources>
```

### [Specify the theme](https://developer.android.com/training/wearables/apps/splash-screen#specify-theme)
In your app's manifest file (`AndroidManifest.xml`), replace the theme of the starting activity - usually the ones that define a launcher item or are otherwise exported - to the theme you created in the previous step:
```
<manifest>
    <application android:theme="@style/Theme.App.Starting">
       <!-- or -->
       <activity android:theme="@style/Theme.App.Starting">
          <!-- ... -->
</manifest>
```

### [Update your starting activity](https://developer.android.com/training/wearables/apps/splash-screen#update-starting-activity)
Install your splash screen in the starting activity before calling `super.onCreate()`:
```
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        // Handle the splash screen transition.
        installSplashScreen()

        super.onCreate(savedInstanceState)
        setContent {
            WearApp("Wear OS app")
        }
    }
}
```

## [Adapt your custom splash screen Activity to the splash screen](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate#best-practices)
After you migrate to the splash screen for Android 12 and later, decide what to do with your previous custom splash screen `Activity`. You have the following options:
- [Keep the custom activity, but prevent it from displaying](#Prevent-the-custom-Activity-from-being-displayed);
- [Keep the custom activity for branding reasons](#Keep-the-custom-activity-for-branding);
- [Remove the custom activity and adapt your app as needed](#Remove-the-custom-splash-screen-Activity).

### [Prevent the custom Activity from being displayed](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate#prevent)
If your previous splash screen `Activity` is primarily used for routing, consider ways to remove it. For example, you might directly link to the actual activity or move to a singular activity with subcomponents. If this isn't feasible, you can use `SplashScreen.setKeepOnScreenCondition` to keep the routing activity in place but stop it from rendering. Doing so transfers the splash screen to the next activity and supports a smooth transition.

```
 class RoutingActivity : Activity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        val splashScreen = installSplashScreen()
        super.onCreate(savedInstanceState)

        // Keep the splash screen visible for this Activity.
        splashScreen.setKeepOnScreenCondition { true }
        startSomeNextActivity()
        finish()
     }
   ...
```

### [Keep the custom activity for branding](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate#branding)
If you want to use a previous splash screen `Activity` for branding reasons, you can transition from the system splash screen into your custom splash screen `Activity` by [customizing the animation for dismissing the splash screen](https://developer.android.com/about/versions/12/features/splash-screen#customize-animation). However, it's best to avoid this scenario if possible and use the `SplashScreen` API to brand your splash screen.

If you need to display a [dialog](https://developer.android.com/develop/ui/views/components/dialogs), we recommend displaying it over the subsequent custom splash screen activity or over the main activity after the system splash screen.

### [Remove the custom splash screen Activity](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate#remove)
Generally, we recommend removing your previous custom splash screen `Activity` altogether to avoid the duplication of splash screens, to increase efficiency, and to reduce splash screen loading times. There are different techniques that you can use to avoid showing redundant splash screen activities.
- **Use lazy loading for your components, modules, or libraries**. Avoid loading or initializing components or libraries that aren't required for the app to work at launch. Load them later, when the app needs them;
- **Create a placeholder while loading a small amount of data locally**. Use the recommended theming approach and hold back the rendering until the app is ready;
- **Show placeholders**. For network-based loads with indeterminate durations, dismiss the splash screen and show placeholders for asynchronous loading. Consider applying subtle animations to the content area that reflect the loading state;
- **Use caching**. When a user opens your app for the first time, you can show loading indicators for some UI elements, as shown in the following figure. The next time the user returns to your app, you can show this cached content while you load more recent content.

# Links
[Migrate your splash screen implementation to Android 12 and later](https://developer.android.com/develop/ui/views/launch/splash-screen/migrate)

[Splash screens](https://developer.android.com/develop/ui/views/launch/splash-screen)

[Add a splash screen](https://developer.android.com/training/wearables/apps/splash-screen)

# Further reading
[Creating a Splash Screen in Android Using the New Splash Screen API](https://blair49.medium.com/creating-a-splash-screen-in-android-using-the-new-splash-screen-api-290870f9956c)

[Implementing the Perfect Splash Screen in Android](https://medium.com/geekculture/implementing-the-perfect-splash-screen-in-android-295de045a8dc)

# Next questions
[What do you know about app startup time?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20app%20startup%20time.md)

[What do you know about App Startup library?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20App%20Startup%20library.md)
