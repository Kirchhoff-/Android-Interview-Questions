# Security best practices

## Show an app chooser
If an implicit intent can launch at least two possible apps on a user's device, explicitly show an app chooser. This interaction strategy allows users to transfer sensitive information to an app that they trust.

```
val intent = Intent(ACTION_SEND)
val possibleActivitiesList: List<ResolveInfo> =
        queryIntentActivities(intent, PackageManager.MATCH_ALL)

// Verify that an activity in at least two apps on the user's device
// can handle the intent. Otherwise, start the intent only if an app
// on the user's device can handle the intent.
if (possibleActivitiesList.size > 1) {

    // Create intent to show chooser.
    // Title is something similar to "Share this photo with".

    val chooser = resources.getString(R.string.chooser_title).let { title ->
        Intent.createChooser(intent, title)
    }
    startActivity(chooser)
} else if (intent.resolveActivity(packageManager) != null) {
    startActivity(intent)
}
```

## Apply signature-based permissions
When sharing data between two apps that you control or own, use *signature-based* permissions. These permissions don't require user confirmation and instead check that the apps accessing the data are signed using the same signing key. Therefore, these permissions offer a more streamlined, secure user experience.

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <permission android:name="my_custom_permission_name"
                android:protectionLevel="signature" />
```               

## Disallow access to your app's content providers
Unless you intend to send data from your app to a different app that you don't own, you should explicitly disallow other developers' apps from accessing the `ContentProvider` objects that your app contains. This setting is particularly important if your app can be installed on devices running Android 4.1.1 (API level 16) or lower, as the `android:exported` attribute of the `<provider>` element is `true` by default on those versions of Android.

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <application ... >
        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="com.example.myapp.fileprovider"
            ...
            android:exported="false">
            <!-- Place child elements of <provider> here. -->
        </provider>
        ...
    </application>
</manifest>
```

## Use SSL traffic
If your app communicates with a web server that has a certificate issued by a well-known, trusted CA, the HTTPS request is very simple:
```
val url = URL("https://www.google.com")
val urlConnection = url.openConnection() as HttpsURLConnection
urlConnection.connect()
urlConnection.inputStream.use {
    ...
}
```

## Add a network security configuration
If your app uses new or custom CAs, you can declare your network's security settings in a configuration file. This process allows you to create the configuration without modifying any app code.

To add a network security configuration file to your app, follow these steps:
1. Declare the configuration in your app's manifest:
```
<manifest ... >
    <application
        android:networkSecurityConfig="@xml/network_security_config"
        ... >
        <!-- Place child elements of <application> element here. -->
    </application>
</manifest>
```
2. Add an XML resource file, located at `res/xml/network_security_config.xml`.
Specify that all traffic to particular domains should use HTTPS by disabling clear-text:
```
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">secure.example.com</domain>
        ...
    </domain-config>
</network-security-config>
```

## Create your own trust manager
Your SSL checker shouldn't accept every certificate. You may need to set up a trust manager and handle all SSL warnings that occur if one of the following conditions applies to your use case:
- You're communicating with a web server that has a certificate signed by a new or custom CA.
- That CA isn't trusted by the device you're using.
- You cannot use a network security configuration.

## WebView
Whenever possible, load only allowlisted content in `WebView` objects. In other words, the `WebView` objects in your app shouldn't allow users to navigate to sites that are outside of your control.

In addition, you should never enable JavaScript interface support unless you completely control and trust the content in your app's `WebView` objects.

If your app must use JavaScript interface support on devices running Android 6.0 (API level 23) and higher, use HTML message channels instead of communicate between a website and your app, as shown in the following code snippet:

```
val myWebView: WebView = findViewById(R.id.webview)

// messagePorts[0] and messagePorts[1] represent the two ports.
// They are already tangled to each other and have been started.
val channel: Array<out WebMessagePort> = myWebView.createWebMessageChannel()

// Create handler for channel[0] to receive messages.
channel[0].setWebMessageCallback(object : WebMessagePort.WebMessageCallback() {

    override fun onMessage(port: WebMessagePort, message: WebMessage) {
        Log.d(TAG, "On port $port, received this message: $message")
    }
})

// Send a message from channel[1] to channel[0].
channel[1].postMessage(WebMessage("My secure message"))
```

## User data
- **Store private data within internal storage**. Store all private user data within the device's internal storage, which is sandboxed per app. Your app doesn't need to request permission to view these files, and other apps cannot access the files. As an added security measure, when the user uninstalls an app, the device deletes all files that the app saved within internal storage.
- **Store data in external storage based on use case**. Use external storage for large, non-sensitive files that are specific to your app, as well as files that your app shares with other apps.
- **Check validity of data**. If your app uses data from external storage, make sure that the contents of the data haven't been corrupted or modified. Your app should also include logic to handle files that are no longer in a stable format.
- **Store only non-sensitive data in cache files**. To provide quicker access to non-sensitive app data, store it in the device's cache. For caches larger than 1 MB in size, use `getExternalCacheDir()`; otherwise, use `getCacheDir()`. Each method provides you with the `File` object that contains your app's cached data.
- **Use SharedPreferences in private mode**. When using `getSharedPreferences()` to create or access your app's `SharedPreferences` objects, use `MODE_PRIVATE`. That way, only your app can access the information within the shared preferences file.

## Other
- **Code Obfuscation**. Protect the source code by making it unintelligible for both humans and decompiler. All this, while preserving its entire operations during the compilation. The purpose of the obfuscation process is to give an impenetrable code. It promotes the confidentiality of all intellectual properties against reverse engineering.
- **Data encryption**. Mobile app security involves securing all kinds of stored data on the mobile device. It includes the source code as well as the data transmitted between the application and the back-end server. The execution of certificate pinning helps affirm the backend Web service certificate for the application. High-level data encryption is one of the best android mobile app security practices. It protects the valuable data from hackers.
- **Regular Updation And Testing**. Hackers detect vulnerabilities in software and exploit, while developers repair the breach, which causes hackers to discover another weakness. Although Google cannot avoid the development of these vulnerabilities, it effectively updates the Android OS to counter the detected problems. However, these measures will not be useful if the software is not up-to-date. Penetration testing is another method for server-side checks.

## Links
https://developer.android.com/topic/security/best-practices  
https://developer.android.com/training/articles/security-tips  
https://www.rishabhsoft.com/blog/android-app-security-best-practices  
https://quickbirdstudios.com/blog/android-app-security-best-practices/
