# PendingIntent

A `PendingIntent` specifies an action to take in the future. It lets you pass a future `Intent` to another application and allow that application to execute that `Intent` as if it had the same permissions as your application, whether or not your application is still around when the `Intent` is eventually invoked. 

By giving a `PendingIntent` to another application, you are granting it the right to perform the operation you have specified as if the other application was yourself (with the same permissions and identity). As such, you should be careful about how you build the `PendingIntent`: often, for example, the base `Intent` you supply will have the component name explicitly set to one of your own components, to ensure it is ultimately sent there and nowhere else.

It is an `Intent` action that you want to perform but at a later time. The reason it’s needed is because an `Intent` must be created and launched from a valid context in your application, but there are certain cases where one is not available at the time you want to run the action because you are technically outside the application’s context. 

Android 12 includes [important changes](https://developer.android.com/about/versions/12/behavior-changes-12#pending-intent-mutability)  to pending intents, including a change that requires explicitly deciding when a `PendingIntent` is mutable or immutable.

## Example of usage
The most common, and most basic, way to use a `PendingIntent` is as the action associated with a notification:

```
val intent = Intent(applicationContext, MainActivity::class.java).apply {
    action = NOTIFICATION_ACTION
    data = deepLink
}
val pendingIntent = PendingIntent.getActivity(
    applicationContext,
    NOTIFICATION_REQUEST_CODE,
    intent,
    PendingIntent.FLAG_IMMUTABLE
)
val notification = NotificationCompat.Builder(
        applicationContext,
        NOTIFICATION_CHANNEL
    ).apply {
        // ...
        setContentIntent(pendingIntent)
        // ...
    }.build()
notificationManager.notify(
    NOTIFICATION_TAG,
    NOTIFICATION_ID,
    notification
)
```

We can see that we’re constructing a standard type of `Intent` that will open our app, and then simply wrapping that in a `PendingIntent` before adding it to our notification.

In this case, since we have an exact action we know we want to perform, we construct a `PendingIntent` that cannot be modified by the app we pass it to by utilizing a flag called `FLAG_IMMUTABLE`.

## Updating an immutable PendingIntent
If an app needs to update a `PendingIntent` that it needs to be mutable, but that’s not always the case! The app that creates a `PendingIntent` can always update it by passing the flag `FLAG_UPDATE_CURRENT`:
```
val updatedIntent = Intent(applicationContext, MainActivity::class.java).apply {
   action = NOTIFICATION_ACTION
   data = differentDeepLink
}
// Because we're passing `FLAG_UPDATE_CURRENT`, this updates
// the existing PendingIntent with the changes we made above.
val updatedPendingIntent = PendingIntent.getActivity(
   applicationContext,
   NOTIFICATION_REQUEST_CODE,
   updatedIntent,
   PendingIntent.FLAG_IMMUTABLE or PendingIntent.FLAG_UPDATE_CURRENT
)
// The PendingIntent has been updated.
```

## Details on flags

- `FLAG_IMMUTABLE`: Indicates the `Intent` inside the `PendingIntent` cannot be modified by other apps that pass an `Intent` to `PendingIntent.send()`. An app can always use `FLAG_UPDATE_CURRENT` to modify its own `PendingIntents` . Prior to Android 12, a `PendingIntent` created without this flag was mutable by default. On Android versions prior to Android 6 (API 23), `PendingIntents` are always mutable.

- `FLAG_UPDATE_CURRENT`: Requests that the system update the existing `PendingIntent` with the new extra data, rather than storing a new `PendingIntent`. If the `PendingIntent` isn’t registered, then this one will be.

- `FLAG_ONE_SHOT`: Only allows the `PendingIntent` to be sent once (via `PendingIntent.send()`). This can be important when passing a `PendingIntent` to another app if the `Intent` inside it should only be able to be sent a single time. This may be related to convenience, or to prevent the app from performing some action multiple times.

- `FLAG_CANCEL_CURRENT`: Cancels an existing `PendingIntent`, if one exists, before registering this new one. This can be important if a particular `PendingIntent` was sent to one app, and you’d like to send it to a different app, potentially updating the data. By using `FLAG_CANCEL_CURRENT`, the first app would no longer be able to call send on it, but the second app would be.

## Receiving PendingIntents
Sometimes the system or other frameworks will provide a `PendingIntent` as a return from an API call. One example is the method `MediaStore.createWriteRequest()` that was added in Android 11.

```
static fun MediaStore.createWriteRequest(
    resolver: ContentResolver, 
    uris: MutableCollection<Uri>
): PendingIntent
```

Just as a `PendingIntent` created by our app is run with our app’s identity, a `PendingIntent` created by the system is run with the system’s identity. In the case of this API, this allows our app to start an `Activity` that can grant write permission to a collection of Uris to our app.

# Links
[PendingIntent](https://developer.android.com/reference/android/app/PendingIntent)

[PendingIntent in Android Application Development](https://medium.com/programming-lite/pendingintent-in-android-application-development-61818c90458b)

[Android: Pending Intents](https://www.bradcypert.com/android-pending-intents/)

[All About PendingIntents](https://medium.com/androiddevelopers/all-about-pendingintents-748c8eb8619)
