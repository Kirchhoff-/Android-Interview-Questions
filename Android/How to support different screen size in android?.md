# How to support different screen sizes in Android?
Android devices come in all shapes and sizes, so your app's layout needs to be flexible. That is, instead of defining your layout with rigid dimensions that assume a certain screen size and aspect ratio(width to the height of a screen), your layout should gracefully respond to different screen sizes and orientations.

By supporting as many screens as possible, your app can be made available to the greatest number of users with different devices, using a single APK. Additionally, making your app flexible for different screen sizes ensures that your app can handle window configuration changes on the device, such as when the user enables multi-window mode.

You can provide support to different screen sizes by the following techniques:-  
(The following won't necessarily support Android form factors as Android Wear, TV, Auto, and Chrome OS devices)   
## Use view dimensions that allow the layout to resize

##### Use ConstraintLayout
The best way to create a responsive layout for different screen sizes is to use ConstraintLayout as the base layout in your UI. ConstraintLayout allows you to specify the position and size for each view according to spatial relationships with other views in the layout. This way, all the views can move and stretch together as the screen size changes.

The easiest way to build a layout with ConstraintLayout is to use the Layout Editor in Android Studio. It allows you to drag new views to the layout, attach their constraints to the parent view and other sibling views, and edit the view's properties, all without editing any XML by hand.

![Constraint Layout](https://github.com/2tanayk/Android-Interview-Questions/blob/master/Android/res/layout-editor_2x.png)
(no matter what layout you use, you should always avoid hard-coding layout sizes)
###### note:-
To ensure that your layout is flexible and adapts to different screen sizes, you should use "wrap_content" and "match_parent" for the width and height of most view components, instead of hard-coded sizes.
"wrap_content" tells the view to set its size to whatever is necessary to fit the content within that view.
"match_parent" makes the view expand to as much as possible within the parent view

## Create alternative layouts
If the above option doesn't meet your needs,say you need to support a much larger device like tabs,then your app should also provide alternative layout resources to optimize the UI design for certain screen sizes.(As indicated in the image below)
![](https://github.com/2tanayk/Android-Interview-Questions/blob/master/Android/res/sizes-phone-tablet_2x.png)

You can provide screen-specific layouts by creating additional res/layout/ directories—one for each screen configuration that requires a different layout—and then append a screen configuration qualifier to the layout directory name (such as layout-w600dp for screens that have 600dp of available width).
These configuration qualifiers represent the visible screen space available for your app UI. The system takes into account any system decorations (such as the navigation bar) and window configuration changes (such as when the user enables multi-window mode) when selecting the layout from your app.

###### To create an alternative layout in Android Studio (using version 3.0 or higher), proceed as follows:
1. Open your default layout and then click Orientation for Preview in the toolbar.
2. In the drop-down list, click to create a suggested variant such as Create Landscape Variant or click Create Other.
3. If you selected Create Other, the Select Resource Directory appears. Here, select a screen qualifier on the left and add it to the list of Chosen qualifiers. When you're done adding qualifiers, click OK.  

This creates a duplicate layout file in the appropriate layout directory so you can begin customizing the layout for that screen variant.

##### Use the smallest width qualifier
The "smallest width" screen size qualifier allows you to provide alternative layouts for screens that have a minimum width measured in density-independent pixels(dp or dip).
for example  
`res/layout/main_activity.xml           # For handsets (smaller than 600dp available width)`  
`res/layout-sw600dp/main_activity.xml    # For 7” tablets (600dp wide and bigger)`

##### Use the available width qualifier  
Instead of changing the layout based on the smallest width of the screen, you might want to change your layout based on how much width or height is currently available. For example, if you have a two-pane layout, you might want to use that whenever the screen provides at least 600dp of width, which might change depending on whether the device is in landscape or portrait orientation
for example
`res/layout/main_activity.xml         # For handsets (smaller than 600dp available width)`  
`res/layout-w600dp/main_activity.xml  # For 7” tablets or any screen with 600dp available width (possibly landscape handsets)`  
(If the available height is a concern for you, then you can do the same using the "available height" qualifier. For example, layout-h600dp for screens with at least 600dp of screen height.)

##### Modularize UI components with fragments
When designing your app for multiple screen sizes you want to make sure you aren't needlessly duplicating your UI behavior across your activities. So you should use fragments to extract your UI logic into separate components. Then, you can then combine fragments to create multi-pane layouts when running on a large screen or place in separate activities when running on a handset.

For example, a news app on a tablet might show a list of articles on the left side and a full article on the right side—selecting an article on the left updates the article view on the right. On a handset, however, these two components should appear on separate screens—selecting an article from a list changes the entire screen to show that article.

## Create stretchable nine-patch bitmaps
If you use a bitmap as the background in a view that changes size, you will notice Android scales your images as the view grows or shrinks based on the size of the screen or content in the view. This often leads to blurring of the images.Therefore we solve this problem using nine-patch bitmaps(specially formatted PNG files that indicate which areas can and cannot be stretched with a .9.png extension)

A nine-patch bitmap is basically a standard PNG file, but with an extra 1px border that indicates which pixels should be stretched.As shown in figure below,the intersection between the black lines on the left and top edge is the area of the bitmap that can be stretched.

![](https://github.com/2tanayk/Android-Interview-Questions/blob/master/Android/res/ninepatch_raw.png)

When you apply a nine-patch as the background to a view, the framework stretches the image correctly to accommodate it.For more information refer [this](https://developer.android.com/studio/write/draw9patch).

Further on an ending note one should test his/her app on a variety of devices with different screen sizes.

### References
[https://developer.android.com/training/multiscreen/screensizes](https://developer.android.com/training/multiscreen/screensizes)
                                     
                                     
