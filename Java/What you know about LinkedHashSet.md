# LinkedHashSet
Hash table and linked list implementation of the `Set` interface, with predictable iteration order. This implementation differs from `HashSet` in that it maintains a doubly-linked list running through all of its entries. This linked list defines the iteration ordering, which is the order in which elements were inserted into the set (*insertion-order*). Note that insertion order is *not* affected if an element is *re-inserted* into the set. (An element `e` is reinserted into a set `s` if `s.add(e)` is invoked when `s.contains(e)` would return `true` immediately prior to the invocation.)

This implementation spares its clients from the unspecified, generally chaotic ordering provided by `HashSet`, without incurring the increased cost associated with `TreeSet`. It can be used to produce a copy of a set that has the same order as the original, regardless of the original set's implementation:

```
void foo(Set s) {
     Set copy = new LinkedHashSet(s);
         ...
}
```

This technique is particularly useful if a module takes a set on input, copies it, and later returns results whose order is determined by that of the copy. (Clients generally appreciate having things returned in the same order they were presented.)

This class provides all of the optional `Set` operations, and permits `null` elements. Like `HashSet`, it provides constant-time performance for the basic operations (`add`, `contains` and `remove`), assuming the hash function disperses elements properly among the buckets. Performance is likely to be just slightly below that of `HashSet`, due to the added expense of maintaining the linked list, with one exception: Iteration over a `LinkedHashSet` requires time proportional to the *size* of the set, regardless of its capacity. Iteration over a `HashSet` is likely to be more expensive, requiring time proportional to its capacity.

A linked hash set has two parameters that affect its performance: *initial capacity* and *load factor*. They are defined precisely as for `HashSet`. Note, however, that the penalty for choosing an excessively high value for initial capacity is less severe for this class than for `HashSet`, as iteration times for this class are unaffected by capacity.

## Initial capacity
The initial capacity means the number of buckets (in backing `HashMap`) when `LinkedHashSet` is created. The number of buckets will be automatically increased if the current size gets full.

Default initial capacity is 16. We can override this default capacity by passing default capacity in it’s constructor `LinkedHashSet(int initialCapacity)`.

## Load factor
The load factor is a measure of how full the LinkedHashSet is allowed to get before its capacity is automatically increased. Default load factor is 0.75.

This is called threshold and is equal to (DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY). When `LinkedHashSet` elements count exceed this threshold, `LinkedHashSet` is resized and new capacity is double the previous capacity.

With default `LinkedHashSet`, the internal capacity is 16 and load factor is 0.75. The number of buckets will automatically get increased when the table has 12 elements in it.

## Synchronization
`LinkedHashSet` is not synchronized. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally. This is typically accomplished by synchronizing on some object that naturally encapsulates the set. If no such object exists, the set should be "wrapped" using the `Collections.synchronizedSet` method. This is best done at creation time, to prevent accidental unsynchronized access to the set:

`Set s = Collections.synchronizedSet(new LinkedHashSet(...));`

The iterators returned by this class's iterator method are fail-fast: if the set is modified at any time after the iterator is created, in any way except through the iterator's own `remove` method, the `Iterator` throws a `ConcurrentModificationException`. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw `ConcurrentModificationException` on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: *the fail-fast behavior of iterators should be used only to detect bugs*.

## Example of usage

```
import java.util.LinkedHashSet;

public class LinkedHashSetExample {

  public static void main(String[]args) {
    LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>();

    linkedHashSet.add("India");
    linkedHashSet.add("Australia");
    linkedHashSet.add("South Africa");
    linkedHashSet.add("India");// adding duplicate elements
    linkedHashSet.add("Ukraine");
    linkedHashSet.add("Usa");
    linkedHashSet.add("France");

    System.out.println("Initial LinkedHashSet:");
    System.out.println(linkedHashSet);

    linkedHashSet.remove("Australia");
    linkedHashSet.remove("France");

    System.out.println("LinkedHashSet after removing elements:");
    System.out.println(linkedHashSet);
  }
}
```

Output:
```
Initial LinkedHashSet: 
[India, Australia, South Africa, Ukraine, Usa, France]
LinkedHashSet after removing elements: 
[India, South Africa, Ukraine, Usa]
```

## Conclusion
`LinkedHashSet` are:
- Extends `HashSet` class which extends `AbstractSet` class;
- Implements `Set` interface;
- Implements Searlizable and Cloneable interfaces;
- Duplicate values are not allowed;
- Allowed `null` elements;
- It is an ordered collection which is the order in which elements were inserted into the set (insertion-order);
- Not synchronized;
- The initial default capacity is 16, and the load factor is 0.75;
- Time complexity average - `O(1)`
- Time complexity worse - `O(n)`

# Links
https://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashSet.html  
https://howtodoinjava.com/java/collections/java-linkedhashset/
