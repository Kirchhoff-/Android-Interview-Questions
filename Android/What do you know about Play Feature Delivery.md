# Play Feature Delivery
Google Play’s app serving model uses [Android App Bundles](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20App%20Bundles.md) to generate and serve optimized APKs for each user’s device configuration, so users download only the code and resources they need to run your app.

Play Feature Delivery uses advanced capabilities of app bundles, allowing certain features of your app to be delivered conditionally or downloaded on demand. To do that, first you need to separate these features from your base app into feature modules.

## [Feature module build configuration](https://developer.android.com/guide/playcore/feature-delivery#feature_build_config)
When you create a new feature module using Android Studio, the IDE applies the following Gradle plugin to the module’s `build.gradle` file:
```
// The following applies the dynamic-feature plugin to your feature module.
// The plugin includes the Gradle tasks and properties required to configure and build
// an app bundle that includes your feature module.

plugins {
  id 'com.android.dynamic-feature'
}
```
Many of the properties available to [the standard application plugin](https://developer.android.com/studio/build#module-level) are also available to your feature module. The following sections describe the properties you should and shouldn’t include in your feature module’s build configuration.

### [What not to include in the feature module build configuration](https://developer.android.com/guide/playcore/feature-delivery)
Because each feature module depends on the base module, it also inherits certain configurations. So, you should omit the following in the feature module’s `build.gradle` file:

- **Signing configurations**: App bundles are signed using signing configurations that you specify in the base module;
- **The `minifyEnabled` property**: You can [enable code shrinking](https://developer.android.com/studio/build/shrink-code#shrink-code) for your entire app project from only the base module’s build configuration. So, you should omit this property from feature modules. You can, however, [specify additional ProGuard rules](https://developer.android.com/guide/playcore/feature-delivery#dynamic_feature_proguard) for each feature module.
- **`versionCode` and `versionName`**: When building your app bundle, Gradle uses app version information that the base module provides. You should omit these properties from your feature module’s `build.gradle` file.

### [Establish a relationship to the base module](https://developer.android.com/guide/playcore/feature-delivery#base_feature_relationship)
When Android Studio creates your feature module, it makes it visible to the base module by adding the `android.dynamicFeatures` property to the base module’s `build.gradle` file, as shown below:
```
// In the base module’s build.gradle file.
android {
    ...
    // Specifies feature modules that have a dependency on
    // this base module.
    dynamicFeatures = [":dynamic_feature", ":dynamic_feature2"]
}
```

Additionally, Android Studio includes the base module as a dependency of the feature module, as shown below:
```
// In the feature module’s build.gradle file:
...
dependencies {
    ...
    // Declares a dependency on the base module, ':app'.
    implementation project(':app')
}
```

## [Use feature modules for custom delivery](https://developer.android.com/guide/playcore/feature-delivery#customize_delivery)
A unique benefit of feature modules is the ability to customize how and when different features of your app are downloaded onto devices running Android 5.0 (API level 21) or higher. For example, to reduce the initial download size of your app, you can configure certain features to be either downloaded as needed on demand or only by devices that support certain capabilities, such as the ability to take pictures or support augmented reality features.

Although you get highly optimized downloads by default when you upload your app as an app bundle, the more advanced and customizable feature delivery options require additional configuration and modularization of your app’s features using feature modules. That is, feature modules provide the building blocks for creating modular features that you can configure to each be downloaded as needed.

Consider an app that allows your users to buy and sell goods in an online marketplace. You can reasonably modularize each of the following functionalities of the app into separate feature modules:
- Account login and creation;
- Browsing the marketplace;
- Placing an item for sale;
- Processing payments.

## [Considerations for feature modules](https://developer.android.com/guide/playcore/feature-delivery#considerations)
With feature modules, you can improve build speed and engineering velocity and extensively customize delivery of your app's features to reduce your app's size. However, there are some constraints and edge cases to keep in mind when using feature modules:

- Installing 50 or more feature modules on a single device, via conditional or on-demand delivery, might lead to performance issues. Install-time modules, which are not configured as removable, are automatically included in the base module and only count as one feature module on each device;
- Limit the number of modules you configure as removable for [install-time delivery](https://developer.android.com/guide/playcore/feature-delivery/install-time) to 10 or fewer. Otherwise, the download and install time of your app might increase;
- Only devices running Android 5.0 (API level 21) and higher support downloading and installing features on demand. To make your feature available to earlier versions of Android, enable **Fusing** when you create a feature module;
- Enable [SplitCompat](https://developer.android.com/reference/com/google/android/play/core/splitcompat/SplitCompat), so that your app has access to downloaded feature modules that are delivered on demand;
- Feature modules should not specify activities in their manifest with `android:exported` set to `true`. That's because there's no guarantee that the device has downloaded the feature module when another app tries to launch the activity. Additionally, your app should confirm that a feature is downloaded before trying to access its code and resources. To learn more, read [Manage installed modules](https://developer.android.com/guide/playcore/feature-delivery/on-demand#manage_installed_modules);
- Because Play Feature Delivery requires you to publish your app using an app bundle, make sure that you're aware of app bundle [known issues](https://developer.android.com/guide/app-bundle#known_issues).

# Links
[Overview of Play Feature Delivery](https://developer.android.com/guide/playcore/feature-delivery)
