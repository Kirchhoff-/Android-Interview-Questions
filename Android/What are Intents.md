## Intent

An Intent is basically a message that is passed between components (such as Activities, Services, Broadcast Receivers, and Content Providers). So, it is almost equivalent to parameters passed to API calls. The fundamental differences between API calls and invoking components via intents are:

* API calls are synchronous while intent-based invocations are asynchronous.
* API calls are compile-time binding while intent-based calls are run-time binding.

## Explicit Intents

Used to call a specific component. When you know which component you want to launch and you do not want to give the user free control over which component to use.

## Implicit Intents

Implicit intents do not name a specific component, but instead declare a general action to perform, which allows a component from another app to handle it. For example, if you want to show the user a location on a map, you can use an implicit intent to request that another capable app show a specified location on a map.

The Android system then compares the given intent against all apps installed on the device to see which ones can handle that action, and therefore process that intent. If more than one can handle the intent, the user is prompted to choose one
If only one app responds, the intent automatically takes the user to that app to perform the action.


## Links
https://developer.android.com/reference/android/content/Intent
https://www.vogella.com/tutorials/AndroidIntent/article.html  
https://www.raywenderlich.com/4700198-android-intents-tutorial-with-kotlin
