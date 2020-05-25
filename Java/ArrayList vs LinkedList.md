# ArrayList vs LinkedList

Internally, `ArrayList` is using an array to implement the List `interfac`e. As arrays are fixed size in Java, `ArrayList` creates an array with some initial capacity. Along the way, if we need to store more items than that default capacity, it will replace that array with a new and more spacious one.  When more and more elements are added into an ArrayList, the capacity of the underlying array grows 50% of its size each time. Internally, a new array which is 1.5 times the size the original array is allocated, and the old array is copied to the new one.

`LinkedList`, as its name suggests, uses a collection of linked nodes to store and retrieve elements. Each node maintains two pointers: one pointing to the next element and another referring to the previous one. `LinkedList` allows for constant-time insertions or removals using iterators. However, it allows only sequential access of elements.

## Comparing ArrayList and LinkedList

Both ArrayList and LinkedList are similar to use. The main difference is their implementation that gives different performances in different operations. The major differences between the two are:

- **Random access of elements:** `ArrayList` allows fast and random access of elements as it is essentially an array that works on index basis. Its elements can be directly accessed using the get and set methods. Whereas in `LinkedList`, finding an element’s position in the list takes time proportional to the size of the list. Any indexed operation requires a traversal.
- **Random insertion and deletion**: As `LinkedList` uses doubly linked list, it takes constant time for insertions or removals as it does not require bit shifting in memory. On the other hand, adding or removing anywhere from an `ArrayList` except at the end requires shifting all the latter elements over, either to make an opening or fill to the gap.
- **Insertion and deletion from head**: Inserting or deleting elements from the head is cheaper in `LinkedList` than `ArrayList`.
- **Queue functionality**: `ArrayList` can act only as list but `LinkedList` can act both as list and queue as it implements the `List` and `Deque` interfaces.
- **Memory overhead**: Memory overhead in `LinkedList` is more as compared to `ArrayList` as a node in `LinkedList` needs to maintain the addresses of next and previous nodes. Whereas an `ArrayList` doesn’t have this overhead as in an `ArrayList` each index only holds the actual object (data).
- **Size**: An `ArrayList` take up as much memory as is allocated for the capacity, regardless of whether elements have actually been added or not. The default initial capacity of an `ArrayList` is pretty small. But since the underlying implementation is an array, the array must be resized if you add a lot of elements. To avoid the high cost of resizing, when you know you’re going to add a lot of elements, construct the `ArrayList` with a higher initial capacity.
- **Reverse iterator**: `LinkedList` can be iterated in reverse direction using `descendingIterator()` while there is no `descendingIterator()` in ArrayList. For reverse iteration, you need to write your own implementation code.

This table shows the time complexity comparisons between various ArrayList and LinkedList operations using Big O notation.

| Operation  | ArrayList | LinkedList |
|---|---|---|
| `get(int index)`  | Runs in constant time i.e O(1) |  Runs proportionately to the amount of data because it has to traverse the list from the beginning or end (whichever is closer) to get to the n-th element. A time complexity of O(n), on average. However, for index =0, it is O(1)  |
| `add(E element)`  | Adds to the end of list. Comes with memory resizing cost. O(1). However, it is O(n) in worst case if internal array is full. This happens because there is extra cost of resizing the array and copying elements to the new array.  | Adds to the end of the list. O(1)  |
| `add(int index, E element)`  | Adds to the specific index position. Requires shifting and possible memory resizing cost if internal array is filled. O(n)  | O(n) but O(1) when index = 0 |
| `remove(int index)` | O(n) | O(n) |
| `Iterator.remove()` | O(n) | O(1) |
| `ListIterator.add(E element)` | O(n) | O(1) |

## Conclusion

| ArrayList  | LinkedList|
|---|---|
| Internally uses a dynamic array to store the elements  | Internally uses a doubly linked list to store the elements  |
| Manipulation is slow because it internally uses an array. If any element is removed from the array, all the bits are shifted in memory  | Manipulation is faster than `ArrayList` because it uses a doubly linked list, so no bit shifting is required in memory.  |
| Can act as a list only because it implements `List` only.  | Can act as a list and queue both because it implements `List` and `Deque` interfaces. |
| Better for storing and accessing data  |  Better for manipulating data  |

## Links
https://www.baeldung.com/java-arraylist-linkedlist  
https://www.javatpoint.com/difference-between-arraylist-and-linkedlist  
https://springframework.guru/java-arraylist-vs-linkedlist/
