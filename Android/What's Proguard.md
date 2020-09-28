# Proguard
Proguard is free Java class file shrinker, optimizer, obfuscator, and preverifier. It detects and removes unused classes, fields, methods, and attributes. It optimizes bytecode and removes unused instructions. It renames the remaining classes, fields, and methods using short meaningless names.

When you build you project using `Android Gradle plugin 3.4.0` or higher, the plugin no longer uses ProGuard to perform compile-time code optimization. Instead, the plugin works with the R8 compiler to handle the following compile-time tasks:
- **Code shrinking (or tree-shaking)**: detects and safely removes unused classes, fields, methods, and attributes from your app and its library dependencies (making it a valuable tool for working around the 64k reference limit). For example, if you use only a few APIs of a library dependency, shrinking can identify library code that your app is not using and remove only that code from your app.
- **Resource shrinking**: Removes unused resources from your packaged app, including unused resources in your app’s library dependencies. It works in conjunction with code shrinking such that once unused code has been removed, any resources no longer referenced can be safely removed as well. 
- **Obfuscation**: Shortens the name of classes and members, which results in reduced DEX file sizes.
- **Optimization**: Inspects and rewrites your code to further reduce the size of your app’s DEX files. For example, if R8 detects that the `else {}` branch for a given `if/else` statement is never taken, R8 removes the code for the `else {}` branch.

When building the release version of your app, by default, R8 automatically performs the compile-time tasks described above for you. However, you can disable certain tasks or customize R8’s behavior through ProGuard rules files. In fact, R8 works with all of your existing ProGuard rules files, so updating the Android Gradle plugin to use R8 should not require you to change your existing rules.

To enable shrinking, obfuscation, and optimization, include the following in your project-level build.gradle file:
```
android {
    buildTypes {
        release {
            // Enables code shrinking, obfuscation, and optimization for only
            // your project's release build type.
            minifyEnabled true

            // Enables resource shrinking, which is performed by the
            // Android Gradle plugin.
            shrinkResources true

            // Includes the default ProGuard rules files that are packaged with
            // the Android Gradle plugin. To learn more, go to the section about
            // R8 configuration files.
            proguardFiles getDefaultProguardFile(
                    'proguard-android-optimize.txt'),
                    'proguard-rules.pro'
        }
    }
    ...
}
```

## Customize which code to keep
For most situations, the default ProGuard rules file (`proguard-android- optimize.txt`) is sufficient for R8 to remove only the unused code. However, some situations are difficult for R8 to analyze correctly and it might remove code your app actually needs. Some examples of when it might incorrectly remove code include:
- When your app calls a method from the Java Native Interface (JNI)
- When your app looks up code at runtime (such as with reflection)

To fix errors and force R8 to keep certain code, add a `-keep` line in the ProGuard rules file. For example:
```
-keep public class MyClass
```

Alternatively, you can add the `@Keep` annotation to the code you want to keep. Adding `@Keep` on a class keeps the entire class as-is. Adding it on a method or field will keep the method/field (and its name) as well as the class name intact. **Note** that this annotation is available only when using the `AndroidX Annotations Library` and when you include the ProGuard rules file that is packaged with the `Android Gradle plugin`

## Conclusion
Benefits:

Proguard (or R8 compiler) obfuscates your code by removing unused code and renaming classes, fields, and methods with semantically obscure names which make the code base, smaller and more efficient. The result is a smaller sized `.apk` file that is more difficult to reverse engineer.

Drawbacks:
- Potential misconfiguration causes the app to get crash.
- Additional testing is required
- Stacktraces are difficult to read with obfuscated method names.
- `ClassNotFoundExceptions`, which happens when Proguard strips away an entire class that application calls.


## Links
https://www.perfomatix.com/proguard-android  
https://developer.android.com/studio/build/shrink-code
