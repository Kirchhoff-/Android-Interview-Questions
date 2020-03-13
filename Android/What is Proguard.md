# Overdraw

Proguard is free Java class file shrinker, optimizer, obfuscator, and preverifier. It detects and removes unused classes, fields, methods, and attributes. It optimizes bytecode and removes unused instructions. It renames the remaining classes, fields, and methods using short meaningless names.

Main function: 
- Shrinking: detects and safely removes unused classes, fields, methods, and attributes from your app and its library dependencies (making it a valuable tool for working around the 64k reference limit). For example, if you use only a few APIs of a library dependency, shrinking can identify library code that your app is not using and remove only that code from your app. 
- Resource shrinking: removes unused resources from your packaged app, including unused resources in your app’s library dependencies. It works in conjunction with code shrinking such that once unused code has been removed, any resources no longer referenced can be safely removed as well.
- Optimization:  inspects and rewrites your code to further reduce the size of your app’s DEX files.
- Obfuscation:  shortens the name of classes and members, which results in reduced DEX file sizes.

**Notes**: When you build you project using Android Gradle plugin 3.4.0 or higher, the plugin no longer uses ProGuard to perform compile-time code optimization. Instead, the plugin works with the R8 compiler. by default, R8 automatically performs the compile-time tasks described above for you. However, you can disable certain tasks or customize R8’s behavior through ProGuard rules files. In fact, R8 works with all of your existing ProGuard rules files, so updating the Android Gradle plugin to use R8 should not require you to change your existing rules.

## Links
https://developer.android.com/studio/build/shrink-code  
https://www.perfomatix.com/proguard-android/  
https://medium.com/androiddevelopers/practical-proguard-rules-examples-5640a3907dc9
