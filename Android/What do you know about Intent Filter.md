# Intent Filter
An intent filter is an expression in an app's manifest file that specifies the type of intents that the component would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise, if you do *not* declare any intent filters for an activity, then it can be started only with an explicit intent.<sup>[1](https://developer.android.com/guide/components/intents-filters#:~:text=An%20intent%20filter%20is,with%20an%20explicit%20intent.)</sup>

An app component can have any number of intent filters (defined with the `<intent-filter>` element), each one describing a different capability of that component.<sup>[2](https://developer.android.com/guide/topics/manifest/manifest-intro#ifs:~:text=An%20app%20component%20can%20have%20any%20number%20of%20intent%20filters%20(defined%20with%20the%20%3Cintent%2Dfilter%3E%20element)%2C%20each%20one%20describing%20a%20different%20capability%20of%20that%20component.)</sup>

## [`<intent-filter>`](https://developer.android.com/guide/topics/manifest/intent-filter-element)
**Syntax**:
```
<intent-filter android:icon="drawable resource"
               android:label="string resource"
               android:priority="integer" >
    ...
</intent-filter>
```

**Contained in**:
- `<activity>`;
- `<activity-alias>`;
- `<service>`;
- `<receiver>`;
- `<provider>`.

**Must contain**:
- `<action>`.

**Can contain**:
- `<category>`;
- `<data>`.

**Attributes**:
- `android:icon`. An icon that represents the parent activity, service, or broadcast receiver when that component is presented to the user as having the capability described by the filter.
  
  This attribute is set as a reference to a drawable resource containing the image definition. The default value is the icon set by the parent component's `icon` attribute. If the parent doesn't specify an icon, the default is the icon set by the `<application>` element.
  
- `android:label`. A user-readable label for the parent component. This label, rather than the one set by the parent component, is used when the component is presented to the user as having the capability described by the filter.

  The label is set as a reference to a string resource so that it can be localized like other strings in the user interface. However, as a convenience while you're developing the application, it can also be set as a raw string.

  The default value is the label set by the parent component. If the parent doesn't specify a label, the default is the label set by the `<application>` element's `label` attribute.

- `android:priority`. The priority given to the parent component with regard to handling intents of the type described by the filter. This attribute has meaning for both activities and broadcast receivers.

  Use this attribute only if you need to impose a specific order in which the broadcasts are received or want to force Android to prefer one activity over others.

- `android:order`. The order in which the filter is processed when multiple filters match. `order` differs from `priority` in that `priority` applies across apps, while order disambiguates multiple matching filters in a single app. When multiple filters can match, use a directed intent instead. Introduced in API level 28.

- `android:autoVerify`. Whether Android needs to verify that the Digital Asset Links JSON file from the specified host matches this application. Introduced in API level 23.

## [`<data>`](https://developer.android.com/guide/topics/manifest/data-element)
**Syntax**:
```
<data android:scheme="string"
      android:host="string"
      android:port="string"
      android:path="string"
      android:pathPattern="string"
      android:pathPrefix="string"
      android:pathSuffix="string"
      android:pathAdvancedPattern="string"
      android:mimeType="string" />
```

**Contained in**: `<intent-filter>`.

Adds a data specification to an intent filter. The specification is a data type, using the `mimeType` attribute, a URI, or both a data type and a URI. A URI is specified by separate attributes for each of its parts:
```
<scheme>://<host>:<port>[<path>|<pathPrefix>|<pathPattern>|<pathAdvancedPattern>|<pathSuffix>]
```

These attributes that specify the URI format are optional, but also mutually dependent:
- If a `scheme` isn't specified for the intent filter, all the other URI attributes are ignored;
- If a host isn't specified for the filter, the port attribute and all the path attributes are ignored.

All the `<data>` elements contained within the same `<intent-filter>` element contribute to the same filter. So, for example, the following filter specification:
```
<intent-filter . . . >
    <data android:scheme="something" android:host="project.example.com" />
    ...
</intent-filter>
```

is equivalent to this one:
```
<intent-filter . . . >
    <data android:scheme="something" />
    <data android:host="project.example.com" />
    ...
</intent-filter>
```

You can place any number of `<data>` elements inside an `<intent-filter>` to give it multiple data options. None of its attributes have default values.

## [`<category>`](https://developer.android.com/guide/topics/manifest/category-element)
**Syntax**:
```
<category android:name="string" />
```

**Contained in**: `<intent-filter>`.

**Attributes**:
- `android:name`. The name of the category. Standard categories are defined in the `Intent` class as `CATEGORY_name` constants. The name assigned here is derived from those constants by prefixing `android.intent.category`. to the `*name*` that follows `CATEGORY_`. For example, the string value for `CATEGORY_LAUNCHER` is `android.intent.category.LAUNCHER`. For custom categories, use the package name as a prefix so that they are unique.

## [`<action>`](https://developer.android.com/guide/topics/manifest/action-element)
**Syntax**:
```
<category android:name="string" />
```

**Contained in**: `<intent-filter>`.

**Description**: Adds an action to an intent filter. An <intent-filter> element must contain one or more <action> elements. If there are no <action> elements in an intent filter, the filter doesn't accept any Intent objects. 

**Attributes**:
- `android:name`. The name of the action. Some standard actions are defined in the `Intent` class as `ACTION_string` constants. To assign one of these actions to this attribute, prepend `android.intent.action`. to the `string` that follows `ACTION_`. For example, for `ACTION_MAIN`, use `android.intent.action.MAIN`, and for `ACTION_WEB_SEARCH`, use `android.intent.action.WEB_SEARCH`.

For actions you define, it's best to use your app's package name as a prefix to help ensure uniqueness. For example, a `TRANSMOGRIFY` action might be specified as follows:
```
<action android:name="com.example.project.TRANSMOGRIFY" />
```

## [Example filters](https://developer.android.com/guide/components/intents-filters#ExampleFilters)
To demonstrate some of the intent filter behaviors, here is an example from the manifest file of a social-sharing app:
```
<activity android:name="MainActivity" android:exported="true">
    <!-- This activity is the main entry, should appear in app launcher -->
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

<activity android:name="ShareActivity" android:exported="false">
    <!-- This activity handles "SEND" actions with text data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
    <!-- This activity also handles "SEND" and "SEND_MULTIPLE" with media data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <action android:name="android.intent.action.SEND_MULTIPLE"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="application/vnd.google.panorama360+jpg"/>
        <data android:mimeType="image/*"/>
        <data android:mimeType="video/*"/>
    </intent-filter>
</activity>
```

The first activity, `MainActivity`, is the app's main entry point—the activity that opens when the user initially launches the app with the launcher icon:
- The `ACTION_MAIN` action indicates this is the main entry point and does not expect any intent data;
- The `CATEGORY_LAUNCHER` category indicates that this activity's icon should be placed in the system's app launcher.

These two must be paired together in order for the activity to appear in the app launcher.

The second activity, `ShareActivity`, is intended to facilitate sharing text and media content. Although users might enter this activity by navigating to it from `MainActivity`, they can also enter `ShareActivity` directly from another app that issues an implicit intent matching one of the two intent filters.

## [Usecases of `<intent-filter>`](https://medium.com/@manuaravindpta/intentfilter-in-android-c375935ec387#:~:text=Usecases%20of%20Intentfilter)
Intent filters in Android have various use cases that enable your app to interact with other apps, system components, and handle different types of intents. Here are some common use cases of Intent Filters in Android:
- **Deep Linking**: You can use intent filters to enable deep linking into specific activities of your app. By defining a specific URL scheme and associating it with your activity in the intent filter, users can open your app directly from a web link or another app;
- **Implicit Intent Handling**: Intent filters allow your app to respond to implicit intents sent by other apps. For example, if your app can handle the `ACTION_SEND` intent action and `text/plain` data type, users can share text from other apps directly to your app;
- **Broadcast Receivers**: Intent filters are commonly used with `BroadcastReceiver` to listen for specific broadcast intents and take appropriate actions. For instance, you can use an intent filter to listen for the `android.intent.action.BATTERY_LOW` broadcast to perform certain tasks when the device’s battery is running low;
- **Opening Specific File Types**: By specifying data types in intent filters, your app can handle requests to open specific file types, such as images, videos, or documents. Users can then select your app as the default handler for those file types;
- **URL Handling**: If your app interacts with specific URLs or web services, you can define an intent filter to handle those URLs, enabling your app to open and process the data accordingly;
- **App Widget Configuration**: Intent filters can be used to configure app widgets. When users add your app’s widget to their home screen, they can customize the widget’s behavior through a configuration activity launched via an intent filter;
- **Notification Actions**: When users interact with a notification, you can use intent filters to define actions that should be triggered when specific buttons in the notification are tapped;
- **Implicit App Invocations**: Intent filters can enable your app to handle specific system events or actions. For example, your app can handle the `android.intent.action.BOOT_COMPLETED` broadcast to perform tasks when the device boots up;
- **Sharing Content**: Intent filters allow your app to be a content provider for other apps. For instance, if your app has user-generated content, other apps can request that content using a custom intent;
- **Voice Actions**: Intent filters can be used to handle voice actions initiated by the user, enabling voice interaction with your app.

# Links
[Intents and intent filters](https://developer.android.com/guide/components/intents-filters)

[`<intent-filter>`](https://developer.android.com/guide/topics/manifest/intent-filter-element)

[`<data>`](https://developer.android.com/guide/topics/manifest/data-element)

[`<category>`](https://developer.android.com/guide/topics/manifest/category-element)

[`<action>`](https://developer.android.com/guide/topics/manifest/action-element)

[App manifest overview](https://developer.android.com/guide/topics/manifest/manifest-intro)

[IntentFilter in Android](https://medium.com/@manuaravindpta/intentfilter-in-android-c375935ec387)

# Further reading
[Getting started with Android’s intent filters](https://blog.logrocket.com/android-intent-filters/)

[Intent Filters in Android | Example code](https://tutorial.eyehunts.com/android/intent-filters-android-example/)

[Let’s be explicit about our intent(-filters)](https://medium.com/androiddevelopers/lets-be-explicit-about-our-intent-filters-c5dbe2dbdce0)

# Next questions
[What are Intents?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20are%20Intents.md)

[What is `BroadcastReceiver`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20BroadcastReceiver.md)
