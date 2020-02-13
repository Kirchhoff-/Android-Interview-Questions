# Dialog vs DialogFragment

The `Dialog` class is the base class for dialogs, but you should avoid instantiating `Dialog` directly. Instead, use one of it's subclasses. `Dialogs` are entirely dependent on Activities. If the screen is rotated, the dialog is dismissed. 

A `DialogFragment` is a fragment that displays a dialog window, floating on top of its activity's window. This fragment contains a `Dialog` object, which it displays as appropriate based on the fragment's state. Using `DialogFragment` to manage the dialog ensures that it correctly handles lifecycle events such as when the user presses the Back button or rotates the screen. 

## Links
https://developer.android.com/reference/android/app/Dialog.html
https://developer.android.com/reference/android/app/DialogFragment
https://developer.android.com/guide/topics/ui/dialogs
