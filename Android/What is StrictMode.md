# `StrictMode`

`StrictMode` is a developer tool which detects things you might be doing by accident and brings them to your attention so you can fix them.

`StrictMode` is most commonly used to catch accidental disk or network access on the application's main thread, where UI operations are received and animations take place. Keeping disk and network operations off the main thread makes for much smoother, more responsive applications. By keeping your application's main thread responsive, you also prevent ANR dialogs from being shown to users.<sup>[1](https://developer.android.com/reference/android/os/StrictMode#:~:text=StrictMode%20is%20a,shown%20to%20users.)</sup>

Example code to enable from early in your `Application`, `Activity`, or other application component's `Application.onCreate()` method:
```
override fun onCreate(savedInstanceState: Bundle?) {
     super.onCreate(savedInstanceState)
     StrictMode.setThreadPolicy(
         StrictMode.ThreadPolicy.Builder()
         .detectAll()
         .build()
     )
     StrictMode.setVmPolicy(
         StrictMode.VmPolicy.Builder()
         .detectAll()
         .build()
     )
 }
```

You can decide what should happen when a violation is detected. For example, using `StrictMode.ThreadPolicy.Builder.penaltyLog()` you can watch the output of adb logcat while you use your application to see the violations as they happen.<sup>[2](https://developer.android.com/reference/android/os/StrictMode#:~:text=Example%20code%20to,as%20they%20happen.)</sup>

## [Penalty Types](https://medium.com/@sandeepkella23/strictmode-your-android-apps-watchdog-4c97be188d57#:~:text=.onCreate()%3B%0A%7D-,Penalty%20Types,-StrictMode%20uses%20penalties)
`StrictMode` uses penalties to signal violations:
- `penaltyLog()`. Logs the violation in the system logcat, making it easy to see what went wrong;
- `penaltyDeath()`. A drastic measure, forcing the app to crash on a violation. Useful for catching severe problems immediately;
- `penaltyDialog()`. This would display a dialog to the user (not typically used in production).

**Note**: The exact policies offered by `StrictMode` have evolved over Android versions.

# Links
[StrictMode](https://developer.android.com/reference/android/os/StrictMode)

[StrictMode: Your Android App’s Watchdog](https://medium.com/@sandeepkella23/strictmode-your-android-apps-watchdog-4c97be188d57)

# Further reading
[Smooth Operator: Using StrictMode to make your Android App ANR free](https://riggaroo.dev/smooth-operator-using-strictmode-to-make-your-android-app-anr-free/)

[Android Best Practices: StrictMode](https://code.tutsplus.com/android-best-practices-strictmode--mobile-7581t)

[Raising the Bar with Android StrictMode](https://medium.com/wizeline-mobile/raising-the-bar-with-android-strictmode-7042d8a9e67b)
