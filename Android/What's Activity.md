# Activity
The `Activity` class is a crucial component of an Android app, and the way activities are launched and put together is a fundamental part of the platform's application model. Unlike programming paradigms in which apps are launched with a `main()` method, the Android system initiates code in an `Activity` instance by invoking specific callback methods that correspond to specific stages of its lifecycle.

When one app invokes another, the calling app invokes an activity in the other app, rather than the app as an atomic whole. In this way, the activity serves as the entry point for an app's interaction with the user. You implement an activity as a subclass of the `Activity` class.

An activity provides the window in which the app draws its UI. This window typically fills the screen, but may be smaller than the screen and float on top of other windows. Generally, one activity implements one screen in an app. For instance, one of an app’s activities may implement a *Preferences screen*, while another activity implements a *Select Photo screen*.

Most apps contain multiple screens, which means they comprise multiple activities. Typically, one activity in an app is specified as the *main activity*, which is the first screen to appear when the user launches the app. Each activity can then start another activity in order to perform different actions. For example, the main activity in a simple e-mail app may provide the screen that shows an e-mail inbox. From there, the main activity might launch other activities that provide screens for tasks like writing e-mails and opening individual e-mails.

Although activities work together to form a cohesive user experience in an app, each activity is only loosely bound to the other activities; there are usually minimal dependencies among the activities in an app. In fact, activities often start up activities belonging to other apps. For example, a browser app might launch the Share activity of a social-media app.

To use activities in your app, you must register information about them in the app’s manifest, and you must manage activity lifecycles appropriately. The rest of this document introduces these subjects.

## [Configuring the manifest](https://developer.android.com/guide/components/activities/intro-activities#ctm)

For your app to be able to use activities, you must declare the activities, and certain of their attributes, in the manifest.

### [Declare activities](https://developer.android.com/guide/components/activities/intro-activities#da)
To declare your activity, open your manifest file and add an `<activity>` element as a child of the `<application>` element. For example:

```
<manifest ... >
  <application ... >
      <activity android:name=".ExampleActivity" />
      ...
  </application ... >
  ...
</manifest >
```

The only required attribute for this element is `android:name`, which specifies the class name of the activity. You can also add attributes that define activity characteristics such as label, icon, or UI theme.

## [Configuration Changes](https://developer.android.com/reference/android/app/Activity#configuration-changes)
If the configuration of the device (as defined by the `Resources.Configuration` class) changes, then anything displaying a user interface will need to update to match that configuration. Because Activity is the primary mechanism for interacting with the user, it includes special support for handling configuration changes.

Unless you specify otherwise, a configuration change (such as a change in screen orientation, language, input devices, etc) will cause your current activity to be *destroyed*, going through the normal activity lifecycle process of `onPause()`, `onStop()`, and `onDestroy()` as appropriate. If the activity had been in the foreground or visible to the user, once `onDestroy()` is called in that instance then a new instance of the activity will be created, with whatever savedInstanceState the previous instance had generated from `onSaveInstanceState(Bundle)`.

This is done because any application resource, including layout files, can change based on any configuration value. Thus the only safe way to handle a configuration change is to re-retrieve all resources, including layouts, drawables, and strings. Because activities must already know how to save their state and re-create themselves from that state, this is a convenient way to have an activity restart itself with a new configuration.

In some special cases, you may want to bypass restarting of your activity based on one or more types of configuration changes. This is done with the `android:configChanges` attribute in its manifest. For any types of configuration changes you say that you handle there, you will receive a call to your current activity's `onConfigurationChanged(Configuration)` method instead of being restarted. If a configuration change involves any that you do not handle, however, the activity will still be restarted and `onConfigurationChanged(Configuration)` will not be called.

## [Starting Activities and Getting Results](https://developer.android.com/reference/android/app/Activity#starting-activities-and-getting-results)

The `startActivity(Intent)` method is used to start a new activity, which will be placed at the top of the activity stack. It takes a single argument, an `Intent`, which describes the activity to be executed.

Sometimes you want to get a result back from an activity when it ends. For example, you may start an activity that lets the user pick a person in a list of contacts; when it ends, it returns the person that was selected. To do this, you call the `startActivityForResult(Intent, int)` version with a second integer parameter identifying the call. The result will come back through your `onActivityResult(int, int, Intent)` method.

When an activity exits, it can call `setResult(int)` to return data back to its parent. It must always supply a result code, which can be the standard results RESULT_CANCELED, RESULT_OK, or any custom values starting at RESULT_FIRST_USER. In addition, it can optionally return back an `Intent` containing any additional data it wants. All of this information appears back on the parent's `Activity.onActivityResult()`, along with the integer identifier it originally supplied.

If a child activity fails for any reason (such as crashing), the parent activity will receive a result with the code RESULT_CANCELED.

```
public class MyActivity extends Activity {
     ...

     static final int PICK_CONTACT_REQUEST = 0;

     public boolean onKeyDown(int keyCode, KeyEvent event) {
         if (keyCode == KeyEvent.KEYCODE_DPAD_CENTER) {
             // When the user center presses, let them pick a contact.
             startActivityForResult(
                 new Intent(Intent.ACTION_PICK,
                 new Uri("content://contacts")),
                 PICK_CONTACT_REQUEST);
            return true;
         }
         return false;
     }

     protected void onActivityResult(int requestCode, int resultCode,
             Intent data) {
         if (requestCode == PICK_CONTACT_REQUEST) {
             if (resultCode == RESULT_OK) {
                 // A contact was picked.  Here we will just display it
                 // to the user.
                 startActivity(new Intent(Intent.ACTION_VIEW, data));
             }
         }
     }
}
```

# Links 
[Introduction to Activities](https://developer.android.com/guide/components/activities/intro-activities)

[Activity](https://developer.android.com/reference/android/app/Activity)

# Next questions
[Activity Lifecycle](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/Activity%20Lifecycle.md)

[Launch modes](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/Launch%20modes.md)

[What is savedInstanceState bundle](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20is%20savedInstanceState%20bundle.md)