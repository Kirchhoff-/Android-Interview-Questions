# Widget
Home screen widgets are broadcast receivers which provide interactive components. They are primarily used on the Android home screen. They typically display some kind of data and allow the user to perform actions with them.

Widgets typically fall into one of the following categories:
- **Information widgets**. Typically display a few crucial information elements that are important to a user and track how that information changes over time. Good examples for information widgets are weather widgets, clock widgets or sports score trackers.
- **Collection widgets**. Specialize in displaying multitude elements of the same type, such as a collection of pictures from a gallery app, a collection of articles from a news app or a collection of emails/messages from a communication app.
- **Control widgets**. The main purpose of a control widget is to display often used functions that the user can trigger right from the home screen without having to open the app first. A typical example of control widgets are music app widgets that allow the user to play, pause or skip music tracks from outside the actual music app.
- **Hybrid widgets**. Combine elements of different types. For example a music player widget is primarily a control widget, but also keeps the user informed about what track is currently playing. It essentially combines a control widget with elements of an information widget type.

Steps to create a `Widget`:
- Define a layout file
- Create an XML file which describes the properties of the widget, e.g. size or the fixed update frequency.
- Create a `BroadcastReceiver` which is used to build the user interface of the widget.
- Enter the *Widget* configuration in the *AndroidManifest.xml* file.
- Optional you can specify a configuration *activity* which is called once a new instance of the widget is added to the `widget` host

Lifecycle method:
| Method | Description |
|---|---|
| `onEnabled()`   |  Called the first time an instance of your widget is added to the home screen.  |
| `onDisabled()`  |  Called once the last instance of your widget is removed from the home screen.  |
| `onUpdate()`    |  Called for every update of the widget. Contains the ids of `appWidgetIds` for which an update is needed. |
| `onDeleted()`   |  Widget instance is removed from the home screen. |
| `onReceive()`   |  This is called for every broadcast and before each of the above callback methods. You normally don't need to implement this method because the default AppWidgetProvider implementation filters all App Widget broadcasts and calls the above methods as appropriate. |

## Widget limirations
1. Because widgets live on the home screen, they have to co-exist with the navigation that is established there. This limits the gesture support that is available in a widget compared to a full-screen app. The only gestures available for widgets are:
- Touch
- Vertical swipe

2. Creating the App Widget layout is simple if you're familiar with `Layouts`. However, you must be aware that App Widget layouts are based on `RemoteViews`, which do not support every kind of layout or view widget. A RemoteViews object (and, consequently, an App Widget) can support the following layout classes:
- `FragmeLayout`
- `LinearLayout`
- `RelativeLayout`
- `GridLayout`

And the following widget classes:
- `AnalogClock`
- `Button`
- `Chronometer`
- `ImageButton`
- `ImageView`
- `ProgressBar`
- `TextView`
- `ViewFlipper`
- `ListView`
- `GridView`
- `StackView`
- `AdapterViewFlipper`

3. A widget has the same runtime restrictions as a normal broadcast receiver, i.e., it has only 5 seconds to finish its processing. A receive (widget) should therefore perform time consuming operations in a service and perform the update of the widgets from the service.

## Links
https://developer.android.com/guide/topics/appwidgets/overview  
https://www.vogella.com/tutorials/AndroidWidgets/article.html  
https://medium.com/android-bits/android-widgets-ad3d166458d3  
https://developer.android.com/guide/topics/appwidgets
