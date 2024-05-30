# Intent
An [`Intent`](https://developer.android.com/reference/android/content/Intent) is a messaging object you can use to request an action from another app component. Although intents facilitate communication between components in several ways, there are three fundamental use cases:
- **Starting an activity**. An `Activity` represents a single screen in an app. You can start a new instance of an `Activity` by passing an Intent to `startActivity()`. The `Intent` describes the activity to start and carries any necessary data;
- **Starting a service**. A `Service` is a component that performs operations in the background without a user interface. You can start a service to perform a one-time operation (such as downloading a file) by passing an `Intent` to `startService()`. The `Intent` describes the service to start and carries any necessary data. If the service is designed with a client-server interface, you can bind to the service from another component by passing an `Intent` to `bindService()`;
- **Delivering a broadcast**. A broadcast is a message that any app can receive. The system delivers various broadcasts for system events, such as when the system boots up or the device starts charging. You can deliver a broadcast to other apps by passing an `Intent` to `sendBroadcast()` or `sendOrderedBroadcast()`.<sup>[1](https://developer.android.com/guide/components/intents-filters#:~:text=An%20Intent%20is,or%20sendOrderedBroadcast().)</sup>

## Intent types
There are two types of intents - **Explicit** and **Implicit**.

### Explicit intents
**Explicit intents** specify which component of which application will satisfy the intent, by specifying a full `ComponentName`. You'll typically use an explicit intent to start a component in your own app, because you know the class name of the activity or service you want to start.<sup>[2](https://developer.android.com/guide/components/intents-filters#:~:text=Explicit%20intents%20specify%20which%20component%20of%20which%20application%20will%20satisfy%20the%20intent%2C%20by%20specifying%20a%20full%20ComponentName.%20You%27ll%20typically%20use%20an%20explicit%20intent%20to%20start%20a%20component%20in%20your%20own%20app%2C%20because%20you%20know%20the%20class%20name%20of%20the%20activity%20or%20service%20you%20want%20to%20start.)</sup>

**Example**: Starting a specific activity within your app:
```
val intent = Intent(applicationContext, MainActivity::class.java)
startActivity(intent)
```

**Example**: if you built a service in your app, named `DownloadService`, designed to download a file from the web, you can start it with the following code:<sup>[3](https://developer.android.com/guide/components/intents-filters#:~:text=if%20you%20built%20a%20service%20in%20your%20app%2C%20named%20DownloadService%2C%20designed%20to%20download%20a%20file%20from%20the%20web%2C%20you%20can%20start%20it%20with%20the%20following%20code%3A)</sup> 
```
// Executed in an Activity, so 'this' is the Context
// The fileUrl is a string URL, such as "http://www.example.com/image.png"
val downloadIntent = Intent(this, DownloadService::class.java).apply {
    data = Uri.parse(fileUrl)
}
startService(downloadIntent)
```

### Implicit intents
**Implicit intents** do not name a specific component, but instead declare a general action to perform, which allows a component from another app to handle it. For example, if you want to show the user a location on a map, you can use an implicit intent to request that another capable app show a specified location on a map.<sup>[4](https://developer.android.com/guide/components/intents-filters#:~:text=Implicit%20intents%20do,on%20a%20map.)</sup>

When you use an implicit intent, the Android system finds the appropriate component to start by comparing the contents of the intent to the *intent filters* declared in the manifest file of other apps on the device. If the intent matches an intent filter, the system starts that component and delivers it the `Intent` object. If multiple intent filters are compatible, the system displays a dialog so the user can pick which app to use.<sup>[5](https://developer.android.com/guide/components/intents-filters#:~:text=When%20you%20use,app%20to%20use.)</sup>

**Example**: Create a new alarm clock:
```
fun createAlarm(message: String, hour: Int, minutes: Int) {
    val intent = Intent(AlarmClock.ACTION_SET_ALARM).apply {
        putExtra(AlarmClock.EXTRA_MESSAGE, message)
        putExtra(AlarmClock.EXTRA_HOUR, hour)
        putExtra(AlarmClock.EXTRA_MINUTES, minutes)
    }
    if (intent.resolveActivity(packageManager) != null) {
        startActivity(intent)
    }
}
```

**Example**: Add a new event to calendar:
```
fun addEvent(title: String, location: String, begin: Long, end: Long) {
    val intent = Intent(Intent.ACTION_INSERT).apply {
        data = Events.CONTENT_URI
        putExtra(Events.TITLE, title)
        putExtra(Events.EVENT_LOCATION, location)
        putExtra(CalendarContract.EXTRA_EVENT_BEGIN_TIME, begin)
        putExtra(CalendarContract.EXTRA_EVENT_END_TIME, end)
    }
    if (intent.resolveActivity(packageManager) != null) {
        startActivity(intent)
    }
}
```

## [Building an intent](https://developer.android.com/guide/components/intents-filters#Building)
An `Intent` object carries information that the Android system uses to determine which component to start (such as the exact component name or component category that should receive the intent), plus information that the recipient component uses in order to properly perform the action (such as the action to take and the data to act upon).

The primary information contained in an `Intent` is the following:
- [Component name](#Component-name);
- [Action](#Action);
- [Data](#Data);
- [Category](#Category);
- [Extras](#Extras);
- [Flags](#Flags).

### [Component name](https://developer.android.com/guide/components/intents-filters#:~:text=is%20the%20following%3A-,Component%20name,-The%20name%20of)
The name of the component to start.
This is optional, but it's the critical piece of information that makes an intent *explicit*, meaning that the intent should be delivered only to the app component defined by the component name. Without a component name, the intent is *implicit* and the system decides which component should receive the intent based on the other intent information (such as the action, data, and category—described below). If you need to start a specific component in your app, you should specify the component name.

### [Action](https://developer.android.com/guide/components/intents-filters#:~:text=A%20string%20that%20specifies%20the%20generic)
A string that specifies the generic action to perform (such as *view* or *pick*).

In the case of a broadcast intent, this is the action that took place and is being reported. The action largely determines how the rest of the intent is structured—particularly the information that is contained in the data and extras.

You can specify your own actions for use by intents within your app (or for use by other apps to invoke components in your app), but you usually specify action constants defined by the `Intent` class or other framework classes. Here are some common actions for starting an activity:
- `ACTION_VIEW`: Use this action in an intent with `startActivity()` when you have some information that an activity can show to the user, such as a photo to view in a gallery app, or an address to view in a map app;
- `ACTION_SEND`: Also known as the *share* intent, you should use this in an intent with `startActivity()` when you have some data that the user can share through another app, such as an email app or social sharing app.

### [Data](https://developer.android.com/guide/components/intents-filters#:~:text=Data-,The%20URI,-(a%20Uri%20object) )
The URI (a `Uri` object) that references the data to be acted on and/or the MIME type of that data. The type of data supplied is generally dictated by the intent's action. For example, if the action is `ACTION_EDIT`, the data should contain the URI of the document to edit.

When creating an intent, it's often important to specify the type of data (its MIME type) in addition to its URI. For example, an activity that's able to display images probably won't be able to play an audio file, even though the URI formats could be similar. Specifying the MIME type of your data helps the Android system find the best component to receive your intent.

### [Category](https://developer.android.com/guide/components/intents-filters#:~:text=and%20MIME%20type.-,Category,-A%20string%20containing)
A string containing additional information about the kind of component that should handle the intent. Any number of category descriptions can be placed in an intent, but most intents do not require a category. Here are some common categories:
- `CATEGORY_BROWSABLE`: The target activity allows itself to be started by a web browser to display data referenced by a link, such as an image or an e-mail message;
- `CATEGORY_LAUNCHER`: The activity is the initial activity of a task and is listed in the system's application launcher.

### [Extras](https://developer.android.com/guide/components/intents-filters#:~:text=the%20following%20information%3A-,Extras,-Key%2Dvalue%20pairs)
Key-value pairs that carry additional information required to accomplish the requested action. Just as some actions use particular kinds of data URIs, some actions also use particular extras.
You can add extra data with various `putExtra()` methods, each accepting two parameters: the key name and the value. You can also create a `Bundle` object with all the extra data, then insert the `Bundle` in the `Intent` with `putExtras()`.

For example, when creating an intent to send an email with `ACTION_SEND`, you can specify the to recipient with the `EXTRA_EMAIL` key, and specify the *subject* with the `EXTRA_SUBJECT` key.

### [Flags](https://developer.android.com/guide/components/intents-filters#:~:text=Flags-,Flags%20are,-defined%20in%20the)
Flags are defined in the `Intent` class that function as metadata for the intent. The flags may instruct the Android system how to launch an activity (for example, which task the activity should belong to) and how to treat it after it's launched (for example, whether it belongs in the list of recent activities).

# Links
[Intents and intent filters](https://developer.android.com/guide/components/intents-filters)

# Next questions
[What are the main components of an android application?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/Main%20components%20of%20Android%20application.md)

[What do you know about Intent Filter?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20do%20you%20know%20about%20Intent%20Filter.md)

[What do you know about URI?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20do%20you%20know%20about%20URI.md)

[What launch modes do you know?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20launch%20modes%20do%20you%20know.md)

# Further Reading
[Common intents](https://developer.android.com/guide/components/intents-common)

[Using Intents in 7 Steps | What is Intent? Why Should I Know About It?](https://medium.com/huawei-developers/using-intents-in-7-steps-what-is-intent-why-should-i-know-about-it-d65bb8eeea4f)
