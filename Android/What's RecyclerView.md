# RecyclerView
The `RecyclerView` class supports the display of a collection of data. You supply the data and define how each item looks, and the RecyclerView library dynamically creates the elements when they're needed.

`RecyclerView` *recycles* those individual elements. When an item scrolls off the screen, `RecyclerView` doesn't destroy its view. Instead, `RecyclerView` reuses the view for new items that have scrolled onscreen. This reuse vastly improves performance, improving your app's responsiveness and reducing power consumption.

## Key classes

Several different classes work together to build your dynamic list.
- `RecyclerView` is the `ViewGroup` that contains the views corresponding to your data. It's a view itself, so you add `RecyclerView` into your layout the way you would add any other UI element.
- Each individual element in the list is defined by a *view holder* object. When the *view holder* is created, it doesn't have any data associated with it. After the view holder is created, the `RecyclerView` binds it to its data. You define the view holder by extending `RecyclerView.ViewHolder`.
- The `RecyclerView` requests those views, and binds the views to their data, by calling methods in the *adapter*. You define the adapter by extending `RecyclerView.Adapter`.
- The *layout manager* arranges the individual elements in your list. You can use one of the layout managers provided by the RecyclerView library, or you can define your own. Layout managers are all based on the library's `LayoutManager` abstract class.

## Steps for implementing your RecyclerView
- Decide what the list or grid is going to look like. Ordinarily you'll be able to use one of the RecyclerView library's standard layout managers.
- Design how each element in the list is going to look and behave. Based on this design, extend the `ViewHolder` class. Your version of `ViewHolder` provides all the functionality for your list items. Your view holder is a wrapper around a `View`, and that view is managed by `RecyclerView`.
- Define the `Adapte`r that associates your data with the `ViewHolder` views.

## Modifying the layout
The `RecyclerView` uses a layout manager to position the individual items on the screen and determine when to reuse item views that are no longer visible to the user. To reuse (*or recycle*) a view, a layout manager may ask the adapter to replace the contents of the view with a different element from the dataset. Recycling views in this manner improves performance by avoiding the creation of unnecessary views or performing expensive `findViewById()` lookups. The Android Support Library includes three standard layout managers, each of which offers many customization options:
- `LinearLayoutManager` arranges the items in a one-dimensional list. Using a `RecyclerView` with `LinearLayoutManager` provides functionality like the older `ListView` layout.
- `GridLayoutManager` arranges the items in a two-dimensional grid, like the squares on a checkerboard. Using a `RecyclerView` with `GridLayoutManager` provides functionality like the older `GridView` layout.
- `StaggeredGridLayoutManager` arranges the items in a two-dimensional grid, with each column slightly offset from the one before, like the stars in an American flag.

If none of these layout managers suits your needs, you can create your own by extending the `RecyclerView.LayoutManager` abstract class.

## Implementing your adapter and view holder
Once you've determined your layout, you need to implement your `Adapter` and `ViewHolder`. These two classes work together to define how your data is displayed. The `ViewHolder` is a wrapper around a `View` that contains the layout for an individual item in the list. The `Adapter` creates `ViewHolder` objects as needed, and also sets the data for those views. The process of associating views to their data is called *binding*.

When you define your adapter, you need to override three key methods:
- `onCreateViewHolder()`: `RecyclerView` calls this method whenever it needs to create a new `ViewHolder`. The method creates and initializes the `ViewHolder` and its associated `View`, but does not fill in the view's contents—the `ViewHolder` has not yet been bound to specific data.
- `onBindViewHolder()`: `RecyclerView` calls this method to associate a `ViewHolder` with data. The method fetches the appropriate data and uses the data to fill in the view holder's layout. For example, if the `RecyclerView` dislays a list of names, the method might find the appropriate name in the list and fill in the view holder's `TextView` widget.
- `getItemCount()`: `RecyclerView` calls this method to get the size of the data set. For example, in an address book app, this might be the total number of addresses. `RecyclerView` uses this to determine when there are no more items that can be displayed.

Here's a typical example of a simple adapter with a nested `ViewHolder` that displays a list of data (Kotlin):
```
class CustomAdapter(private val dataSet: Array<String>) :
        RecyclerView.Adapter<CustomAdapter.ViewHolder>() {

    /**
     * Provide a reference to the type of views that you are using
     * (custom ViewHolder).
     */
    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val textView: TextView

        init {
            // Define click listener for the ViewHolder's View.
            textView = view.findViewById(R.id.textView)
        }
    }

    // Create new views (invoked by the layout manager)
    override fun onCreateViewHolder(viewGroup: ViewGroup, viewType: Int): ViewHolder {
        // Create a new view, which defines the UI of the list item
        val view = LayoutInflater.from(viewGroup.context)
                .inflate(R.layout.text_row_item, viewGroup, false)

        return ViewHolder(view)
    }

    // Replace the contents of a view (invoked by the layout manager)
    override fun onBindViewHolder(viewHolder: ViewHolder, position: Int) {

        // Get element from your dataset at this position and replace the
        // contents of the view with that element
        viewHolder.textView.text = dataSet[position]
    }

    // Return the size of your dataset (invoked by the layout manager)
    override fun getItemCount() = dataSet.size

}
```

The layout for the each view item is defined in an XML layout file, as usual. In this case, the app has a `text_row_item.xml` file like this:
```
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="@dimen/list_item_height"
    android:layout_marginLeft="@dimen/margin_medium"
    android:layout_marginRight="@dimen/margin_medium"
    android:gravity="center_vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/element_text"/>
</FrameLayout>
```

# Links
https://developer.android.com/guide/topics/ui/layout/recyclerview  
https://developer.android.com/guide/topics/ui/layout/recyclerview-custom

# Next questions
[How does RecyclerView differ from ListView](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/recyclerview/Android/How%20does%20RecyclerView%20differ%20from%20ListView.md)
