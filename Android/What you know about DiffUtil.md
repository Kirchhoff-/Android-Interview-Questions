# DiffUtil

`DiffUtil` is a utility class that calculates the difference between two lists and outputs a list of update operations that converts the first list into the second one. 

It can be used to calculate updates for a RecyclerView Adapter.  See [ListAdapter](https://developer.android.com/reference/androidx/recyclerview/widget/ListAdapter) and [AsyncListDiffer](https://developer.android.com/reference/androidx/recyclerview/widget/AsyncListDiffer) which can simplify the use of `DiffUtil` on a background thread.

`DiffUtil` uses Eugene W. Myers's difference algorithm to calculate the minimal number of updates to convert one list into another. Myers's algorithm does not handle items that are moved so DiffUtil runs a second pass on the result to detect items that were moved.

Note that `DiffUtil`, `ListAdapter`, and `AsyncListDiffer` require the list to not mutate while in use. This generally means that both the lists themselves and their elements (or at least, the properties of elements used in diffing) should not be modified directly. Instead, new lists should be provided any time content changes. It's common for lists passed to `DiffUtil` to share elements that have not mutated, so it is not strictly required to reload all data to use `DiffUtil`.

If the lists are large, this operation may take significant time so you are advised to run this on a background thread, get the `DiffUtil.DiffResult` then apply it on the `RecyclerView` on the main thread.

## DiffUtil.Callback
`DiffUtil.Callback` class used by `DiffUtil` while calculating the diff between two lists.

| `Method`  | `Description`  |
|---|---|
| `abstract Boolean areContentsTheSame(oldItemPosition: Int, newItemPosition: Int)`  |  Called by the `DiffUtil` when it wants to check whether two items have the same data.  |
| `abstract Boolean areItemsTheSame(oldItemPosition: Int, newItemPosition: Int)`  | Called by the `DiffUtil` to decide whether two object represent the same Item.  |
| `open Any? getChangePayload(oldItemPosition: Int, newItemPosition: Int)`  | When `areItemsTheSame(int, int)` returns `true` for two items and `areContentsTheSame(int, int)` returns false for them, `DiffUtil` calls this method to get a payload about the change.  |
| `abstract Int getNewListSize()`  | Returns the size of the new list. |
| `abstract Int getOldListSize()`  | Returns the size of the old list.  |

# Links
https://developer.android.com/reference/androidx/recyclerview/widget/DiffUtil  
https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/DiffUtil.Callback  
https://medium.com/@iammert/using-diffutil-in-android-recyclerview-bdca8e4fbb00
