# Main components of android application
Application components are the essential building blocks of an Android application. These components are loosely coupled by the application manifest file `AndroidManifest.xml` that describes each component of the application and how they interact.

Main components of android application are:
- Activities
- Services
- Broadcast Receivers
- Content Providers

A **Activity** represents a single screen with a user interface, in-short Activity performs actions on the screen. An activity provides the window in which the app draws its UI. This window typically fills the screen, but may be smaller than the screen and float on top of other windows. Generally, one activity implements one screen in an app. For instance, one of an app’s activities may implement a Preferences screen, while another activity implements a Select Photo screen. In new architectural approaches, there is only one activity for the entire application, and it controls the fragments that interact with the user.

A **Service** is an application component that can perform long-running operations in the background, and it doesn't provide a user interface. Another application component can start a service, and it continues to run in the background even if the user switches to another application. Additionally, a component can bind to a service to interact with it and even perform interprocess communication (IPC). For example, a service can handle network transactions, play music, perform file I/O, or interact with a content provider, all from the background.

A **Broadcast Receivers** (receiver) is an Android component which allows you to register for system or application events. Android apps can send or receive broadcast messages from the Android system and other Android apps, similar to the publish-subscribe design pattern. These broadcasts are sent when an event of interest occurs. For example, the Android system sends broadcasts when various system events occur, such as when the system boots up or the device starts charging. Apps can also send custom broadcasts, for example, to notify other apps of something that they might be interested in (for example, some new data has been downloaded).

A **Content Provider** manages access to a central repository of data. You implement a provider as one or more classes in an Android application, along with elements in the manifest file. One of your classes implements a subclass `ContentProvider`, which is the interface between your provider and other applications. Although content providers are meant to make data available to other applications, you may of course have activities in your application that allow the user to query and modify the data managed by your provider.

## Links   
https://www.tutorialspoint.com/android/android_application_components.htm  
https://developer.android.com/guide/components/activities/intro-activities  
https://developer.android.com/guide/components/services  
https://www.vogella.com/tutorials/AndroidBroadcastReceiver/article.html  
https://developer.android.com/guide/topics/providers/content-provider-creating
