# HashMap works

HashMap in Java works on hashing principles. It is a data structure which allows us to store object and retrieve it in constant time O(1) provided we know the key. In hashing, hash functions are used to link key and value in HashMap.

Before understanding the internal working of HashMap, you must be aware of `hashCode()` and `equals()` method.
- `equals()` - It checks the equality of two objects. It compares the Key, whether they are equal or not. It is a method of the `Object` class. It can be overridden. If you override the `equals(`) method, then it is mandatory to override the `hashCode()` method.
- `hashCode()` - This is the method of the object class. It returns the memory reference of the object in integer form. The value received from the method is used as the bucket number. The bucket number is the address of the element inside the map. Hash code of `null` Key is 0. 

Buckets. A bucket is one element of HashMap array. It is used to store nodes. Two or more nodes can have the same bucket. In that case link list structure is used to connect the nodes. Buckets are different in capacity. A relation between bucket and capacity is as follows:

`capacity = number of buckets * load factor`

A single bucket can have more than one nodes, it depends on `hashCode()` method. The better your `hashCode()` method is, the better your buckets will be utilized.

## Put()/Get()
What put() method does:
- First of all, the `key` object is checked for `null`. If the key is `null`, the `value` is stored in `table[0]` position. Because hashcode for `null` is always 0
- Then on next step, a hash value is calculated using the key’s hash code by calling its `hashCode()` method. This hash value is used to calculate the index in the array for storing `Entry` object. JDK designers well assumed that there might be some poorly written `hashCode()` functions that can return very high or low hash code value. To solve this issue, they introduced another `hash()` function and passed the object’s hash code to this `hash()` function to bring hash value in the range of array index size.
- Now `indexFor(hash, table.length)` function is called to calculate exact index position for storing the `Entry` object.

Same logic is applied in `get()` method. The moment hashmap identify the exact match for the key object passed as an argument, it simply returns the value object stored in current Entry object.

## Collisions 
Two unequal objects can have the same hash code value, so they will be stored in same array location called bucket. For resolve such situation HashMap uses `LinkedList`. `Entry` class had an attribute "next". This attribute always points to the next object in the chain. 

So, in case of collision, `Entry` objects are stored in linked list form. When an `Entry` object needs to be stored in particular index, HashMap checks whether there is already an entry? If there is no entry already present, the entry object is stored in this location. If there is already an object sitting on calculated index, its `next` attribute is checked. If it is `null`, and current entry object becomes next node in linkedlist. If `next` variable is not `null`, procedure is followed until `next` is evaluated as `null`.

If we add the another `value` object with same key as entered before, logically, it should replace the old value. `HashMap` determining the index position of `Entry` object, while iterating over linkedist on calculated index and calls `equals()` method on key object for each entry object. If `key.equals(k)` will be `true` then both keys are treated as same `key` object. This will cause the replacing of value object inside `entry` object only.

## Key notes
- Data structure to store entry objects is an array named `table` of type `Entry`.
- A particular index location in array is referred as bucket, because it can hold the first element of a linkedlist of entry objects.
- Key object’s `hashCode()` is required to calculate the index location of Entry object.
- Key object’s `equals()` method is used to maintain uniqueness of keys in map.
- Value object’s `hashCode()` and `equals()` method are not used in HashMap’s `get()` and `put()` methods.
- Hash code for `null` keys is always zero, and such entry object is always stored in zero index in `Entry[]`.

## Improvements in Java 8
As part of the work for JEP 180, there is a performance improvement for HashMap objects where there are lots of collisions in the keys by using balanced trees rather than linked lists to store map entries. The principal idea is that **once the number of items in a hash bucket grows beyond a certain threshold, that bucket will switch from using a linked list of entries to a balanced tree. In the case of high hash collisions, this will improve worst-case performance from** `O(n)` **to** `O(log n)`.

Basically when a bucket becomes too big (**currently: TREEIFY_THRESHOLD = 8**), HashMap dynamically replaces it with an ad-hoc implementation of the treemap. This way rather than having pessimistic `O(n)` we get much better `O(log n)`.

Bins (elements or nodes) of TreeNodes may be traversed and used like any others, but additionally support faster lookup when overpopulated. However, since the vast majority of bins in normal use are not overpopulated, checking for the existence of tree bins may be delayed in the course of table methods.

Tree bins (i.e., bins whose elements are all TreeNodes) are ordered primarily by hashCode, but in the case of ties, if two elements are of the same `class C implements Comparable<C>`, type then their `compareTo()` method is used for ordering.

Because TreeNodes are about twice the size of regular nodes, we use them only when bins contain enough nodes. And when they become too small (due to removal or resizing) they are converted back to plain bins (**currently: UNTREEIFY_THRESHOLD = 6**). In usages with well-distributed user hashCodes, tree bins are rarely used.

## Links
https://howtodoinjava.com/java/collections/hashmap/how-hashmap-works-in-java/  
https://www.geeksforgeeks.org/internal-working-of-hashmap-java/  
https://www.javatpoint.com/working-of-hashmap-in-java  
https://dzone.com/articles/how-hashmap-works-internally-in-java  
