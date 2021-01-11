# SparseArray
`SparseArray` maps integers to Objects and, unlike a normal array of Objects, its indices can contain gaps. `SparseArray` is intended to be more memory-efficient than a `HashMap`, because it avoids auto-boxing keys and its data structure doesn't rely on an extra entry object for each mapping.

Note that this container keeps its mappings in an array data structure, using a binary search to find keys. The implementation is not intended to be appropriate for data structures that may contain large numbers of items. It is generally slower than a `HashMap` because lookups require a binary search, and adds and removes require inserting and deleting entries in the array. For containers holding up to hundreds of items, the performance difference is less than 50%.

To help with performance, the container includes an optimization when removing keys: instead of compacting its array immediately, it leaves the removed entry marked as deleted. The entry can then be re-used for the same key or compacted later in a single garbage collection of all removed entries. This garbage collection must be performed whenever the array needs to be grown, or when the map size or entry values are retrieved.

It is possible to iterate over the items in this container using `keyAt(int)` and `valueAt(int)`. Iterating over the keys using `keyAt(int)` with ascending values of the index returns the keys in ascending order. In the case of `valueAt(int)`, the values corresponding to the keys are returned in ascending order.

Benefits to use `SparseArray` over `HashMap` is:
- More memory efficient by using primitives;
- No auto-boxing;
- Allocation-free.

Drawbacks:
- For large collections, it is slower;
- It only available for Android.

## Example of usage
```
import android.util.SparseArray
import androidx.core.util.set
import androidx.core.util.size

class SparseArrayExample {

    fun example() {
        val sparseArray = SparseArray<String>()
        sparseArray[0] = "First item"
        sparseArray[10] = "Second item"
        sparseArray[24] = "Third item"
        sparseArray[50] = "Fourth item"

        for (i in 0 until sparseArray.size) {
            println("Index = " + sparseArray.keyAt(i) + ", Item = " + sparseArray.valueAt(i))
        }

        if (sparseArray[100] == null) {
            println("SparseArray not contains item with index 100")
        }
    }
}
```

Output:
```
Index = 0, Item = First item
Index = 10, Item = Second item
Index = 24, Item = Third item
Index = 50, Item = Fourth item
SparseArray not contains item with index 100
```

# Links
https://developer.android.com/reference/android/util/SparseArray  
https://android.jlelse.eu/app-optimization-with-arraymap-sparsearray-in-android-c0b7de22541a

# Next questions
[What you know about ArrayMap](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20you%20know%20about%20ArrayMap.md)