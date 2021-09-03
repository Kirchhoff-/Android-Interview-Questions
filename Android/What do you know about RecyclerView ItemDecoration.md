# RecyclerView.ItemDecoration
An ItemDecoration allows the application to add a special drawing and layout offset to specific item views from the adapter's data set. This can be useful for drawing dividers between items, highlights, visual grouping boundaries and more.

All ItemDecorations are drawn in the order they were added, before the item views(in `onDraw()` and after the items (in `onDrawOver(Canvas, RecyclerView, RecyclerView.State`).

Multiple ItemDecorations can be added to a single `RecyclerView`. 

A few examples of `ItemDecoration`:

`ItemDecoration` for set top and bottom margins:

```
import android.graphics.Rect
import android.view.View
import androidx.recyclerview.widget.RecyclerView

class TopBottomMarginItemDecoration(
    private val topMargin: Int,
    private val bottomMargin: Int
) : RecyclerView.ItemDecoration() {

    override fun getItemOffsets(
        outRect: Rect,
        view: View,
        parent: RecyclerView,
        state: RecyclerView.State
    ) {
        with(outRect) {
            top = topMargin
            bottom = bottomMargin
        }
    }
}
```

`ItemDecoration` for set right and left margins:

```
import android.graphics.Rect
import android.view.View
import androidx.recyclerview.widget.RecyclerView

class EdgesMarginItemDecoration(private val edgesMargin: Int) : RecyclerView.ItemDecoration() {

    override fun getItemOffsets(
        outRect: Rect,
        view: View,
        parent: RecyclerView,
        state: RecyclerView.State
    ) {
        with(outRect) {
            val position = parent.getChildAdapterPosition(view)
            when (position) {
                0 -> {
                    left = edgesMargin
                    right = edgesMargin / 2
                }
                parent.adapter!!.itemCount - 1 -> {
                    right = edgesMargin
                    left = edgesMargin / 2
                }
                else -> {
                    left = edgesMargin / 2
                    right = edgesMargin / 2
                }
            }
        }
    }
}
```

# Links
[RecyclerView.ItemDecoration](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.ItemDecoration)

# Further reading
[ItemDecoration in Android](https://proandroiddev.com/itemdecoration-in-android-e18a0692d848)