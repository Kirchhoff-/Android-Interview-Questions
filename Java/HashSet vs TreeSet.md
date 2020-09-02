# HashSet vs TreeSet

[HashSet](https://github.com/DevelopTest-byte/Md-test/blob/master/Java/HashSet.md) class implements the `Set` interface, backed by a hash table (actually a `HashMap` instance). It makes no guarantees as to the iteration order of the set; in particular, it does not guarantee that the order will remain constant over time. This class permits the `null` element.

This class offers constant time performance for the basic operations (*add*, *remove*, *contains* and *size*), assuming the hash function disperses the elements properly among the buckets. Iterating over this set requires time proportional to the sum of the `HashSet` instance's size (the number of elements) plus the "capacity" of the backing `HashMap` instance (the number of buckets).

[TreeSet](https://github.com/DevelopTest-byte/Md-test/blob/master/Java/TreeSet.md) class implements the `Set` interface that uses a tree for storage. It inherits `AbstractSet` class and implements the `NavigableSet` interface. The elements are ordered using their natural ordering, or by a `Comparator` provided at set creation time, depending on which constructor is used.

`TreeSet` is basically an implementation of a self-balancing binary search tree like a Red-Black Tree. Therefore operations like *add*, *remove*, and *search* take `O(log(n))` time. The reason is that in a self-balancing tree, it is made sure that the height of the tree is always `O(log(n))` for all the operations. Therefore, this is considered as one of the most efficient data structure in order to store the huge sorted data and perform operations on it. However, operations like printing N elements in the sorted order takes `O(n)` time.

| Property  | `HashSet`  | `TreeSet`  |
|---|---|---|
| Ordering        | Does not maintain any order   | Maintains an object in sorted order   |
| Comprasion      | Uses `equals()` method | Uses `compareTo()` method |
| Data structure  | Implemented using HashTable  | Implemented using a tree structure  |
| Null element    | Allowed  | Not allowed  |
| Performance     | Time complexity average - `O(1)` | Guaranteed `O(log(n))` time cost for the basic operations  |

## Links
https://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html  
https://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html  
https://www.geeksforgeeks.org/hashset-in-java/  
https://www.geeksforgeeks.org/treeset-in-java-with-examples/  
https://www.geeksforgeeks.org/hashset-vs-treeset-in-java/  
https://www.tutorialspoint.com/difference-between-tree-set-and-hash-set-in-java
