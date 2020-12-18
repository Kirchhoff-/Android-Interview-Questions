# ArrayMap
`ArrayMap` is a generic key->value mapping data structure that is designed to be more memory efficient than a traditional `HashMap`. It keeps its mappings in an array data structure - an integer array of hash codes for each item, and an Object array of the key/value pairs. This allows it to avoid having to create an extra object for every entry put in to the map, and it also tries to control the growth of the size of these arrays more aggressively (since growing them only requires copying the entries in the array, not rebuilding a hash map).

Note that this implementation is not intended to be appropriate for data structures that may contain large numbers of items. It is generally slower than a traditional `HashMap`, since lookups require a binary search and adds and removes require inserting and deleting entries in the array. For containers holding up to hundreds of items, the performance difference is not significant, less than 50%.

Because this container is intended to better balance memory use, unlike most other standard Java containers it will shrink its array as items are removed from it. Currently you have no control over this shrinking - if you set a capacity and then remove an item, it may reduce the capacity to better match the current size. In the future an explicit call to set the capacity should turn off this aggressive shrinking behavior.

This structure is **NOT** thread-safe.

## How ArrayMap works
`ArrayMap` uses 2 arrays. The instance variables used internally are `Object[] mArray` to store the objects and the `int[] mHashes` to store hashCodes. When a key/value is inserted:
- Key/Value is autoboxed;
- The key object is inserted in the `mArray` where on the index in which it needs to pushed is searched using the binary search;
- A value object is also inserted in the position next to key’s position in `mArray[]`;
- The hashCode of the key is calculated and placed in `mHashes[]` at the next available position.

For searching a key:
- Key’s hashCode is calculated;
- Binary search is done for this hashCode in the `mHashes` array. This implies time complexity increases to `O(logN)`;
- Once we get the index of hash, we know that key is at 2\*index position in `mArray` and value is at 2\*index+1 position;
- Here the time complexity increases from `O(1)` to `O(logN)`, but it is memory efficient. Whenever we play on a dataset of around 100, there will no problem of time complexity, it will be non-noticeable. As we have the advantage of memory efficient application.

## Example of usage
```
import androidx.collection.ArrayMap

class ArrayMapExample {

    fun example() {
        val arrayMap = ArrayMap<String, String>()
        arrayMap["USA"] = "Washington"
        arrayMap["Germany"] = "Berlin"
        arrayMap["France"] = "Paris"
        arrayMap["Italy"] = "Rome"
        arrayMap["Australia"] = "Canberra"

        for (i in 0 until arrayMap.size) {
            println("Country = " + arrayMap.keyAt(i) + ", Capital = " + arrayMap.valueAt(i))
        }
    }
}
```

Output:
```
Country = Australia, Capital = Canberra
Country = USA, Capital = Washington
Country = Italy, Capital = Rome
Country = Germany, Capital = Berlin
Country = France, Capital = Paris
```

# Links
https://developer.android.com/reference/android/util/ArrayMap  
https://medium.com/@mohom.r/optimising-android-app-performance-with-arraymap-9296f4a1f9eb
