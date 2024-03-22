# Reduce your app size
Small app size is directly related to download success, particularly in emerging markets with poor network device connections or low network speeds. This can result in lower app usage rates, which in turn lowers the scope and reach of your audience.<sup>[1](https://developer.android.com/guide/topics/androidgo/optimize-size#:~:text=Small%20app%20size%20is%20directly%20related%20to%20download%20success%2C%20particularly%20in%20emerging%20markets%20with%20poor%20network%20device%20connections%20or%20low%20network%20speeds.%20This%20can%20result%20in%20lower%20app%20usage%20rates%2C%20which%20in%20turn%20lowers%20the%20scope%20and%20reach%20of%20your%20audience.)</sup>

The main ways to reduce the size of app are:
- [Upload your app with Android App Bundles](#Upload-your-app-with-android-app-bundles)
- [Reduce resource count and size](#Reduce-resource-count-and-size)
  - [Remove unused resources](#Remove-unused-resources)
  - [Support only specific densities](#Support-only-specific-densities)
  - [Use drawable objects](#Use-drawable-objects)
  - [Reuse resources](#Reuse-resources)
  - [Compress PNG and JPEG files](#Compress-PNG-and-JPEG-files)
  - [Use WebP file format](#Use-WebP-file-format)
  - [Use Downloadable Fonts](#Use-Downloadable-Fonts)
- [Reduce native and Java code](#Reduce-native-and-Java-code)
- [Re-evaluate feature and translation](#Re-evaluate-feature-and-translation)
  - [Re-evaluate infrequently used features](#Re-evaluate-infrequently-used-features)
  - [Utilize dynamic delivery](#Utilize-dynamic-delivery)
  - [Reduce translatable string size](#Reduce-translatable-string-size)
  - [Use selective translation](#Use-selective-translation)

## [Upload your app with Android App Bundles](https://developer.android.com/topic/performance/reduce-apk-size#app_bundle)
Upload your app as an Android App Bundle to immediately save app size when you publish to Google Play. Android App Bundle is an upload format that includes all your app's compiled code and resources but defers APK generation and signing to Google Play.

Google Play's app serving model then uses your app bundle to generate and serve optimized APKs for each user's device configuration so that they download only the code and resources they need to run your app. You don't have to build, sign, and manage multiple APKs to support different devices, and users get smaller, more optimized downloads.

## [Reduce resource count and size](https://developer.android.com/topic/performance/reduce-apk-size#reduce-resources)
The size of your APK has an impact on how fast your app loads, how much memory it uses, and how much power it consumes. You can make your APK smaller by reducing the number and size of the resources it contains. In particular, you can remove resources that your app no longer uses, and you can use scalable `Drawable` objects in place of image files. 

### [Remove unused resources](https://developer.android.com/topic/performance/reduce-apk-size#remove-unused)
The `lint` tool—a static code analyzer included in Android Studio—detects resources in your `res/` folder that your code doesn't reference. When the `lint` tool discovers a potentially unused resource in your project, it prints a message like the following example:
```
res/layout/preferences.xml: Warning: The resource R.layout.preferences appears
    to be unused [UnusedResources]
```

Libraries that you add to your code might include unused resources. Gradle can automatically remove resources on your behalf if you enable `shrinkResources` in your app's `build.gradle.kts` file:
```
android {
    // Other settings.

    buildTypes {
        getByName("release") {
            minifyEnabled = true
            shrinkResources = true
            proguardFiles(getDefaultProguardFile('proguard-android.txt'), "proguard-rules.pro")
        }
    }
}
```

To use `shrinkResources`, enable code shrinking. During the build process, R8 first removes unused code. Then, the Android Gradle plugin removes the unused resources.

### [Support only specific densities](https://developer.android.com/topic/performance/reduce-apk-size#support-densities)
Android supports different screen densities, such as the following:
- `ldpi`;
- `mdpi`;
- `tvdpi`;
- `hdpi`;
- `xhdpi`;
- `xxhdpi`;
- `xxxhdpi`.

Although Android supports the preceding densities, you don't need to export your rasterized assets to each density.

If you know that only a small percentage of your users have devices with specific densities, consider whether you need to bundle those densities into your app. If you don't include resources for a specific screen density, Android automatically scales existing resources originally designed for other screen densities.

If your app needs only scaled images, you can save even more space by having a single variant of an image in `drawable-nodpi/`. We recommend you include at least an `xxhdpi` image variant in your app.

### [Use drawable objects](https://developer.android.com/topic/performance/reduce-apk-size#use-drawables)
Some images don't require a static image resource. The framework can dynamically draw the image at runtime instead. `Drawable` objects—or `<shape>` in XML — can take up a tiny amount of space in your APK. In addition, XML `Drawable` objects produce monochromatic images compliant with Material Design guidelines.

### [Reuse resources](https://developer.android.com/topic/performance/reduce-apk-size#reuse-resources)
You can include a separate resource for variations of an image, such as tinted, shaded, or rotated versions of the same image. However, we recommend that you reuse the same set of resources and customizing them as needed at runtime.

Android provides several utilities to change the color of an asset, either using the `android:tint` and `tintMode` attributes.

You can also omit resources that are only a rotated equivalent of another resource. The following code snippet provides an example of turning a "thumb up" into a "thumb down" by pivoting at the middle of the image and rotating it 180 degrees:
```
<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:drawable="@drawable/ic_thumb_up"
    android:pivotX="50%"
    android:pivotY="50%"
    android:fromDegrees="180" />
```

### [Compress PNG and JPEG files](https://developer.android.com/topic/performance/reduce-apk-size#compress)
You can reduce PNG file sizes without losing image quality using tools like [pngcrush](https://pmt.sourceforge.io/pngcrush/), [pngquant](https://pngquant.org/), or [zopflipng](https://github.com/google/zopfli). All of these tools can reduce PNG file size while preserving the perceptive image quality.

The `pngcrush` tool is particularly effective. This tool iterates over PNG filters and zlib (Deflate) parameters, using each combination of filters and parameters to compress the image. It then chooses the configuration that yields the smallest compressed output.

To compress JPEG files, you can use tools like [packJPG](https://github.com/packjpg/packJPG) and [guetzli](https://github.com/google/guetzli).

### [Use WebP file format](https://developer.android.com/topic/performance/reduce-apk-size#use-webp)
Instead of using PNG or JPEG files, you can also use the [WebP](https://developers.google.com/speed/webp) file format for your images. The WebP format provides lossy compression and transparency, like JPG and PNG, and it can provide better compression than either JPEG or PNG.

You can convert existing BMP, JPG, PNG or static GIF images to WebP format using Android Studio. For more information, see [Create WebP images](https://developer.android.com/studio/write/convert-webp).

### [Use Downloadable Fonts](https://www.elluminatiinc.com/reduce-android-app-size/#:~:text=Use%20Downloadable%20Fonts)
Since most programs on the Play Store share the same fonts, the App package already includes many of them. Duplication occurs when a user runs multiple apps with the same fonts on the same device. In response to the issue, Google added Downloadable fonts to its Support collection. APIs can now simply request typefaces instead of bundling files.

## [Reduce native and Java code](https://developer.android.com/topic/performance/reduce-apk-size#reduce-code)
You can use the following methods to reduce the size of the Java and native codebase in your app:
- **Remove unnecessary generated code**. Make sure to understand the footprint of any code which is automatically generated. For example, many protocol buffer tools generate an excessive number of methods and classes, which can double or triple the size of your app;
- **Avoid enumerations**. A single enum can add about 1.0 to 1.4 KB to your app's `classes.dex` file. These additions can quickly accumulate for complex systems or shared libraries. If possible, consider using the `@IntDef` annotation and code shrinking to strip enumerations out and convert them to integers. This type conversion preserves all of the type safety benefits of enums;
- **Reduce the size of native binaries**. If your app uses native code and the Android NDK, you can also reduce the size of the release version of your app by optimizing your code. Two useful techniques are removing debug symbols and not extracting native libraries;
- **Remove debug symbols**. Using debug symbols makes sense if your app is in development and still requires debugging. Use the `arm-eabi-strip` tool provided in the Android NDK to remove unnecessary debug symbols from native libraries. Afterwards, you can compile your release build.
- **Avoid extracting native libraries**. When building the release version of your app, package uncompressed `.so` files in the APK by setting `useLegacyPackaging` to `false` in your app's `build.gradle.kts` file. Disabling this flag prevents `PackageManager` from copying `.so` files from the APK to the filesystem during installation. This method makes updates of your app smaller.

## Re-evaluate feature and translation

### [Re-evaluate infrequently used features](https://developer.android.com/guide/topics/androidgo/optimize-size#feature-use)
Specifically optimize for Android (Go edition) by disabling features that have low daily active user (DAU) metrics. Examples of this include removing complex animations, large GIF files, or any other aesthetic additions not necessary for app success.

### [Utilize dynamic delivery](https://developer.android.com/guide/topics/androidgo/optimize-size#dynamic-delivery)
Play Feature Delivery uses advanced capabilities of app bundles, allowing certain features of your app to be delivered conditionally or downloaded on demand. You can use feature modules for custom delivery. A unique benefit of feature modules is the ability to customize how and when different features of your app are downloaded onto devices running Android 5.0 (API level 21) or higher.

### [Reduce translatable string size](https://developer.android.com/guide/topics/androidgo/optimize-size#string-size)
You can use the Android Gradle `resConfigs` property to remove alternative resource files that your app doesn't need. If you're using a library that includes language resources (such as AppCompat or Google Play Services), then your app includes all translated language strings for library messages, regardless of app translation. If you'd like to keep only the languages that your app officially supports, you can specify those languages using the `resConfig` property. Any resources for languages not specified are removed.

To limit your language resources to just English and French, you can edit `defaultConfig` as shown below:
```
android {
    defaultConfig {
        ...
        resConfigs "en", "fr"
    }
}
```

### [Use selective translation](https://developer.android.com/guide/topics/androidgo/optimize-size#translation)
If a given string isn't visible in the app's UI, then you don't have to translate it. Strings for the purpose of debugging, exception messages, or URLs should be string literals in code, not resources.

For example, don't bother translating URLs:
```
<string name="car_frx_device_incompatible_sol_message">
  This device doesn\'t support Android Auto.\n
  &lt;a href="https://support.google.com/androidauto/answer/6395843"&gt;Learn more&lt;/a&gt; 
</string>
```

You may recognize `&lt;` and `&gt`, as these are escape characters for `<` and `>`. They’re needed here because if you were to put an `<a>` tag inside of a `<string>` tag, then the Android resource compiler drops them since it doesn't recognize the tag. However, this means that you’re translating the HTML tags and the URL to 78 languages. Instead, you can remove the HTML:
```
<string name="car_frx_device_incompatible_sol_message">
         This device doesn\'t support Android Auto.
</string>
```

# Links
[Reduce your app size](https://developer.android.com/topic/performance/reduce-apk-size)

[Reduce app size](https://developer.android.com/guide/topics/androidgo/optimize-size)

[8 Ways to Reduce Android App Size During Development Phase?](https://www.elluminatiinc.com/reduce-android-app-size/)

# Further Reading
[8 Best Ways to Reduce Android App Size](https://www.mantralabsglobal.com/blog/reduce-android-app-size/)

[Minimizing APK Size: Techniques for Shrinking Android App Size](https://diegomarcher.medium.com/minimizing-apk-size-techniques-for-shrinking-android-app-size-7a4c5eefbd46)

# Next Questions
[What do you know about App Bundles?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20App%20Bundles.md)

[What do you know about Play Feature Delivery?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20Play%20Feature%20Delivery.md)

[What do you know about Lint?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/ff6c77c42013a56a816a5bb65b6703a41bf074f3/Android/What%20do%20you%20know%20about%20Lint.md)

[What do you know about downloadable fonts?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20downloadable%20fonts.md)
