# TreeMap
A Red-Black tree based `NavigableMap` implementation. The map is sorted according to the natural ordering of its keys, or by a `Comparator` provided at map creation time, depending on which constructor is used.

This implementation provides guaranteed `O(log(n))` time cost for the `containsKey`, `get`, `put` and `remove` operations. Algorithms are adaptations of those in Cormen, Leiserson, and Rivest's Introduction to Algorithms.

Note that the ordering maintained by a tree map, like any sorted map, and whether or not an explicit comparator is provided, must be *consistent with equals* if this sorted map is to correctly implement the `Map` interface. This is so because the `Map` interface is defined in terms of the equals operation, but a sorted map performs all key comparisons using its `compareTo` (or `compare`) method, so two keys that are deemed equal by this method are, from the standpoint of the sorted map, equal.

## Synchronization
If multiple threads access a map concurrently, and at least one of the threads modifies the map structurally, it must be synchronized externally. (A structural modification is any operation that adds or deletes one or more mappings; merely changing the value associated with an existing key is not a structural modification.) This is typically accomplished by synchronizing on some object that naturally encapsulates the map. If no such object exists, the map should be "wrapped" using the `Collections.synchronizedSortedMa`p method. This is best done at creation time, to prevent accidental unsynchronized access to the map:

```
SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));
```

The iterators returned by this class's iterator method are fail-fast: if the set is modified at any time after the iterator is created, in any way except through the iterator's own `remove` method, the `Iterator` throws a `ConcurrentModificationException`. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw `ConcurrentModificationException` on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: *the fail-fast behavior of iterators should be used only to detect bugs*.

## [Internal Implementation of TreeMap](https://www.baeldung.com/java-treemap#internal-implementation-of-treemap)
`TreeMap` implements `NavigableMap` interface and bases its internal working on the principles of red-black trees:
```
public class TreeMap<K,V> extends AbstractMap<K,V>
  implements NavigableMap<K,V>, Cloneable, java.io.Serializable
```

**First of all**, a red-black tree is a data structure that consists of nodes; picture an inverted mango tree with its root in the sky and the branches growing downward. The root will contain the first element added to the tree.

The rule is that starting from the root, any element in the left branch of any node is always less than the element in the node itself. Those on the right are always greater. What defines greater or less than is determined by the natural ordering of the elements or the defined comparator at construction as we saw earlier.

This rule guarantees that the entries of a treemap will always be in sorted and predictable order.

**Secondly**, a red-black tree is a self-balancing binary search tree. This attribute and the above guarantee that basic operations like search, get, put and remove take logarithmic time  `O(log(n))`

Being self-balancing is key here. As we keep inserting and deleting entries, picture the tree growing longer on one edge or shorter on the other.

This would mean that an operation would take a shorter time on the shorter branch and longer time on the branch which is furthest from the root, something we would not want to happen.

Therefore, this is taken care of in the design of red-black trees. For every insertion and deletion, the maximum height of the tree on any edge is maintained at `O(log(n))` i.e. the tree balances itself continuously.

## Example of usage
```
import java.util.Collections;
import java.util.Map;
import java.util.TreeMap;

public class TreeMapExample {

    public void example() {
        System.out.println("Ascending order map:");
        TreeMap<Integer, String> treeMap = new TreeMap<>();
        addItemToTreeMap(treeMap);
        printTreeMap(treeMap);

        System.out.println("Reverse order map:");
        treeMap = new TreeMap<>(Collections.reverseOrder());
        addItemToTreeMap(treeMap);
        printTreeMap(treeMap);
    }

    private void addItemToTreeMap(TreeMap<Integer, String> treeMap) {
        treeMap.put(1, "First item");
        treeMap.put(24, "Second item");
        treeMap.put(50, "Third item");
        treeMap.put(99, "Fourth item");
    }

    private void printTreeMap(TreeMap<Integer, String> treeMap) {
        for (Map.Entry<Integer, String> integerStringEntry : treeMap.entrySet()) {
            System.out.println("Key = " + integerStringEntry.getKey() + ", Value = " + integerStringEntry.getValue());
        }
    }
}
```

Output:
```
Ascending order map:
Key = 1, Value = First item
Key = 24, Value = Second item
Key = 50, Value = Third item
Key = 99, Value = Fourth item
Reverse order map:
Key = 99, Value = Fourth item
Key = 50, Value = Third item
Key = 24, Value = Second item
Key = 1, Value = First item
```

## Conclusion
`TreeMap` are:
- Implements `Map` and `NavigableMap` interfaces and extends `AbstractMap` class;
- Guaranteed  `O(log(n))` time cost for the basic operations;
- Doesn't allow `null` element;
- Not synchronized;
- Maintains ascending order;

# Links
https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html  
https://www.geeksforgeeks.org/treemap-in-java/  
https://www.baeldung.com/java-treemap

# Next questions
[What you know about Comparator interface](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20Comparator%20interface.md)  
[Comparable interface in java](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/Comparable%20interface%20in%20java.md) 
