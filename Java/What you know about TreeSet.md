# TreeSet
`TreeSet` class implements the `Set` interface that uses a tree for storage. It inherits `AbstractSet` class and implements the `NavigableSet` interface. The elements are ordered using their natural ordering, or by a `Comparator` provided at set creation time, depending on which constructor is used.

Ordering maintained by a set (whether or not an explicit comparator is provided) must be *consistent with equals* if it is to correctly implement the `Set` interface. This is so because the `Set` interface is defined in terms of the `equals` operation, but a `TreeSet` instance performs all element comparisons using its `compareTo` (or `compare`) method, so two elements that are deemed equal by this method are, from the standpoint of the set, equal. The behavior of a set is well-defined even if its ordering is inconsistent with equals; it just fails to obey the general contract of the `Set` interface.

`TreeSet` is basically an implementation of a self-balancing binary search tree like a Red-Black Tree. Therefore operations like *add*, *remove*, and *search* take `O(log(n))` time. The reason is that in a self-balancing tree, it is made sure that the height of the tree is always `O(log(n))` for all the operations. Therefore, this is considered as one of the most efficient data structure in order to store the huge sorted data and perform operations on it. However, operations like printing N elements in the sorted order takes `O(n)` time.

## Synchronization
`HashSet` is not synchronized. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally. This is typically accomplished by synchronizing on some object that naturally encapsulates the set. If no such object exists, the set should be "wrapped" using the `Collections.synchronizedSet` method. This is best done at creation time, to prevent accidental unsynchronized access to the set:

`Set s = Collections.synchronizedSet(new TreeSet(...));`

The iterators returned by this class's iterator method are fail-fast: if the set is modified at any time after the iterator is created, in any way except through the iterator's own `remove` method, the `Iterator` throws a `ConcurrentModificationException`. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw `ConcurrentModificationException` on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: *the fail-fast behavior of iterators should be used only to detect bugs*.

## Example of usage
```
import java.util.*; 
  
class TreeSetExample { 
  
    public static void main(String[] args) 
    { 
        TreeSet<String> ts1 = new TreeSet<String>(); 
  
        // Elements are added using add() method 
        ts1.add("A"); 
        ts1.add("B"); 
        ts1.add("C"); 
  
        // Duplicates will not get insert 
        ts1.add("C"); 
  
        // Elements get stored in default natural 
        // Sorting Order(Ascending) 
        System.out.println(ts1); 
    } 
} 
```

Output:
```
[A, B, C]
```

## Conclusion
`TreeSet` are:
- Implements `Set` and `NavigableSet` interfaces and inherits from `AbstractSet`;
- Сontains unique elements;
- Guaranteed `O(log(n))` time cost for the basic operations;
- Doesn't allow `null` element;
- Not synchronized;
- Maintains ascending order;

## Links
https://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html  
https://www.javatpoint.com/java-treeset  
https://www.geeksforgeeks.org/treeset-in-java-with-examples/  
https://www.programiz.com/java-programming/treeset
