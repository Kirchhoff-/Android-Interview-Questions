# View

![](./res/view_hierarchy.png "View hierarchy")

`View` represents the basic building block for user interface components. A View occupies a rectangular area on the screen and is responsible for drawing and event handling. View is the base class for widgets, which are used to create interactive UI components (buttons, text fields, etc.). 

All of the views in a window are arranged in a single tree. You can add views either from code or by specifying a tree of views in one or more XML layout files. There are many specialized subclasses of views that act as controls or are capable of displaying text, images, or other content.

## Using Views

Once you have created a tree of views, there are typically a few types of common operations you may wish to perform: 
- **Set properties**: for example setting the text of a `TextView`. The available properties and the methods that set them will vary among the different subclasses of views. Note that properties that are known at build time can be set in the XML layout files.
- **Set focus**: The framework will handle moving focus in response to user input. To force focus to a specific view, call `requestFocus()`.
- **Set up listeners**: Views allow clients to set listeners that will be notified when something interesting happens to the view. For example, all views will let you set a listener to be notified when the view gains or loses focus. You can register such a listener using `setOnFocusChangeListener(android.view.View.OnFocusChangeListener)`. Other view subclasses offer more specialized listeners. For example, a Button exposes a listener to notify clients when the button is clicked.
- **Set visibility**: You can hide or show views using `setVisibility(int)`.

*Note: The Android framework is responsible for measuring, laying out and drawing views. You should not call methods that perform these actions on views yourself unless you are actually implementing a* `ViewGroup`.

## IDs
Views may have an integer id associated with them. These ids are typically assigned in the layout XML files, and are used to find specific views within the view tree. A common pattern is to:

- Define a Button in the layout file and assign it a unique ID.
```
<Button
     android:id="@+id/my_button"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:text="@string/my_button_text"/>
```

- From the onCreate method of an Activity, find the Button
```
Button myButton = findViewById(R.id.my_button);
```

View IDs need not be unique throughout the tree, but it is good practice to ensure that they are at least unique within the part of the tree you are searching.

## Event Handling and Threading
The basic cycle of a view is as follows:
- An event comes in and is dispatched to the appropriate view. The view handles the event and notifies any listeners.
- If in the course of processing the event, the view's bounds may need to be changed, the view will call `requestLayout()`.
- Similarly, if in the course of processing the event the view's appearance may need to be changed, the view will call `invalidate()`.
- If either `requestLayout()` or `invalidate()` were called, the framework will take care of measuring, laying out, and drawing the tree as appropriate.

*Note: The entire view tree is single threaded. You must always be on the UI thread when calling any method on any view.* If you are doing work on other threads and want to update the state of a view from that thread, you should use a `Handler`.

## Links
https://developer.android.com/reference/android/view/View  
https://proandroiddev.com/the-life-cycle-of-a-view-in-android-6a2c4665b95e  
