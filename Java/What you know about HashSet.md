# HashSet
This class implements the `Set` interface, backed by a hash table (actually a `HashMap` instance). It makes no guarantees as to the iteration order of the set; in particular, it does not guarantee that the order will remain constant over time. This class permits the `null` element.

This class offers constant time performance for the basic operations (*add*, *remove*, *contains* and *size*), assuming the hash function disperses the elements properly among the buckets. Iterating over this set requires time proportional to the sum of the `HashSet` instance's size (the number of elements) plus the "capacity" of the backing `HashMap` instance (the number of buckets).

## Load Factor
For the maintenance of constant time performance, iterating over `HashSet` requires time proportional to the sum of the `HashSet` instance’s size (the number of elements) plus the “capacity” of the backing `HashMap` instance (the number of buckets). Thus, it’s very important not to set the initial capacity too high (or the load factor too low) if iteration performance is important.

**Initial Capacity**: The initial capacity means the number of buckets when hashtable is created. The number of buckets will be automatically increased if the current size gets full.

**Load Factor**: The load factor is a measure of how full the `HashSet` is allowed to get before its capacity is automatically increased. When the number of entries in the hash table exceeds the product of the load factor and the current capacity, the hash table is rehashed (that is, internal data structures are rebuilt) so that the hash table has approximately twice the number of buckets.

Formula of load factor:
```
load factor = Number of stored elements in the table / Size of the hash table 
```

Load factor and initial capacity are two main factors that affect the performance of `HashSet` operations. Load factor of 0.75 provides very effective performance as respect to time and space complexity. If we increase the load factor value more than that then memory overhead will be reduced (because it will decrease internal rebuilding operation) but, it will affect the add and search operation in hashtable. To reduce the rehashing operation we should choose initial capacity wisely. If initial capacity is greater than the maximum number of entries divided by the load factor, no rehash operation will ever occur.

## Synchronization
`HashSet` is not synchronized. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally. This is typically accomplished by synchronizing on some object that naturally encapsulates the set. If no such object exists, the set should be "wrapped" using the `Collections.synchronizedSet` method. This is best done at creation time, to prevent accidental unsynchronized access to the set:

`Set s = Collections.synchronizedSet(new HashSet(...));`

The iterators returned by this class's iterator method are fail-fast: if the set is modified at any time after the iterator is created, in any way except through the iterator's own `remove` method, the `Iterator` throws a `ConcurrentModificationException`. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw `ConcurrentModificationException` on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: *the fail-fast behavior of iterators should be used only to detect bugs*.

## Example of usage
```
import java.util.*; 
  
class Test 
{ 
    public static void main(String[]args) 
    { 
        HashSet<String> h = new HashSet<String>(); 
  
        // Adding elements into HashSet usind add() 
        h.add("India"); 
        h.add("Australia"); 
        h.add("South Africa"); 
        h.add("India");// adding duplicate elements 
  
        // Displaying the HashSet 
        System.out.println(h); 
        System.out.println("List contains India or not:" + 
                           h.contains("India")); 
  
        // Removing items from HashSet using remove() 
        h.remove("Australia"); 
        System.out.println("List after removing Australia:"+h); 
  
        // Iterating over hash set items 
        System.out.println("Iterating over list:"); 
        Iterator<String> i = h.iterator(); 
        while (i.hasNext()) 
            System.out.println(i.next()); 
    } 
} 
```

Output:
```
[South Africa, Australia, India]
List contains India or not:true
List after removing Australia:[South Africa, India]
Iterating over list:
South Africa
India
```

## Conclusion
`HashSet` are:
- Implements `Set` Interface;
- Underlying data structure for `HashSet` is hashtable;
- Duplicate values are not allowed;
- Order of elements are not guaranteed. Objects are inserted based on their hash code;
- `null` elements are allowed;
- Implements `Searlizable` and `Cloneable` interfaces;
- Not synchronized;
- The initial default capacity is 16, and the load factor is 0.75;
- Best approach for search operations;
- Time complexity average - `O(1)`
- Time complexity worse - `O(n)`

## Links
https://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html  
https://www.geeksforgeeks.org/hashset-in-java/  
https://www.javatpoint.com/java-hashset
