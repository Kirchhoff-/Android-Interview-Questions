# Foreground Services

Foreground services perform operations that are noticeable to the user.

Foreground services show a status bar notification, to make users aware that your app is performing a task in the foreground and is consuming system resources.

Examples of apps that use foreground services include the following:
- A music player app that plays music in a foreground service. The notification might show the current song being played;
- A fitness app that records a user's run in a foreground service, after receiving permission from the user. The notification might show the distance that the user has traveled during the current fitness session;

Only use a foreground service when your app needs to perform a task that is noticeable by the user, even when they're not directly interacting with the app.<sup>[1](https://developer.android.com/develop/background-work/services/foreground-services#:~:text=Foreground%20services%20perform,with%20the%20app.)</sup>

Starting in Android 13 (API level 33), users can dismiss the notification associated with a foreground service by default. To do so, users perform a swipe gesture on the notification. Traditionally, the notification isn't dismissed unless the foreground service is either stopped or removed from the foreground.<sup>[2](https://developer.android.com/develop/background-work/services/foreground-services#:~:text=Starting%20in%20Android,from%20the%20foreground.)</sup>

## [Declare foreground services in your manifest](https://developer.android.com/develop/background-work/services/foreground-services#types)
In your app's manifest, declare each of your app's foreground services with a `<service>` element. For each service, use an `android:foregroundServiceType` attribute to declare what kind of work the service does.

For example, if your app creates a foreground service that plays music, you might declare the service like this:
```
<manifest xmlns:android="http://schemas.android.com/apk/res/android" ...>
  <application ...>

    <service
        android:name=".MyMediaPlaybackService"
        android:foregroundServiceType="mediaPlayback"
        android:exported="false">
    </service>
  </application>
</manifest>
```

If your multiple types apply to your service, separate them with the `|` operator. For example, a service that uses the camera and microphone would declare it like this:
```
android:foregroundServiceType="camera|microphone"
```

This list shows the foreground service types to choose from:
- [`camera`](https://developer.android.com/about/versions/14/changes/fgs-types-required#camera);
- [`connectedDevice`](https://developer.android.com/about/versions/14/changes/fgs-types-required#connected-device);
- [`dataSync`](https://developer.android.com/about/versions/14/changes/fgs-types-required#data-sync);
- [`health`](https://developer.android.com/about/versions/14/changes/fgs-types-required#health);
- [`location`](https://developer.android.com/about/versions/14/changes/fgs-types-required#location);
- [`mediaPlayback`](https://developer.android.com/about/versions/14/changes/fgs-types-required#media);
- [`mediaProjection`](https://developer.android.com/about/versions/14/changes/fgs-types-required#media-projection);
- [`microphone`](https://developer.android.com/about/versions/14/changes/fgs-types-required#microphone);
- [`phoneCall`](https://developer.android.com/about/versions/14/changes/fgs-types-required#phone-call);
- [`remoteMessaging`](https://developer.android.com/about/versions/14/changes/fgs-types-required#remote-messaging);
- [`shortService`](https://developer.android.com/about/versions/14/changes/fgs-types-required#short-service);
- [`specialUse`](https://developer.android.com/about/versions/14/changes/fgs-types-required#special-use);
- [`systemExempted`](https://developer.android.com/about/versions/14/changes/fgs-types-required#system-exempted).

## [Start a foreground service](https://developer.android.com/develop/background-work/services/foreground-services#start)
Before you request the system to run a service as a foreground service, start the service itself:
```
val intent = Intent(...) // Build the intent for the service
context.startForegroundService(intent)
```

Inside the service, usually in `onStartCommand()`, you can request that your service run in the foreground. To do so, call `ServiceCompat.startForeground()` (available in androidx-core 1.12 and higher). This method takes the following parameters:
- The service;
- A positive integer that uniquely identifies the notification in the status bar;
- The [`Notification`](https://developer.android.com/reference/android/app/Notification) object itself;
- The [foreground service types](https://developer.android.com/develop/background-work/services/fg-service-types) identifying the work done by the service.

## [Restrictions on starting a foreground service from the background](https://developer.android.com/develop/background-work/services/foreground-services#bg-access-restrictions)
Apps that target Android 12 or higher can't start foreground services while the app is running in the background, except for [a few special cases](https://developer.android.com/develop/background-work/services/foreground-services#background-start-restriction-exemptions). If an app tries to start a foreground service while the app runs in the background, and the foreground service doesn't satisfy one of the exceptional cases, the system throws a `ForegroundServiceStartNotAllowedException`.

In addition, if an app wants to launch a foreground service that needs *while-in-use* permissions (for example, body sensor, camera, microphone, or location permissions), it cannot create the service while the app is in the background, even if the app falls into one of the exemptions from background start restrictions.

## [Restrictions on starting foreground services that need while-in-use permissions](https://developer.android.com/develop/background-work/services/foreground-services#wiu-restrictions)
On Android 14 (API level 34) or higher, there are special situations to be aware of if you're starting a foreground service that needs while-in-use permissions.

If your app targets Android 14 or higher, the operating system checks when you create a foreground service to make sure your app has all the appropriate permissions for that service type. For example, when you create a foreground service of type microphone, the operating system verifies that your app currently has the `RECORD_AUDIO` permission. If you don't have that permission, the system throws a `SecurityException`.

For while-in-use permissions, this causes a potential problem. If your app has a while-in-use permission, it only has that permission *while it's in the foreground*. This means if your app is in the background, and it tries to create a foreground service of type camera, location, or microphone, the system sees that your app doesn't *currently* have the required permissions, and it throws a `SecurityException`.

# Links
[Foreground services](https://developer.android.com/develop/background-work/services/foreground-services)

[Foreground service types are required](https://developer.android.com/about/versions/14/changes/fgs-types-required)

# Further reading
[A guide to using foreground services and background work in Android 14](https://www.droidcon.com/2023/11/15/a-guide-to-using-foreground-services-and-background-work-in-android-14/)

[Guide to Foreground Services on Android 14](https://medium.com/@domen.lanisnik/guide-to-foreground-services-on-android-9d0127dc8f9a)
