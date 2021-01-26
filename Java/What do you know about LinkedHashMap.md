# LinkedHashMap
Hash table and linked list implementation of the `Map` interface, with predictable iteration order. This implementation differs from `HashMap` in that it maintains a doubly-linked list running through all of its entries. This linked list defines the iteration ordering, which is normally the order in which keys were inserted into the map (*insertion-order*). Note that insertion order is not affected if a key is *re-inserted* into the map. (A key `k` is reinserted into a map `m` if `m.put(k, v)` is invoked when `m.containsKey(k)` would return `true` immediately prior to the invocation.) 

This implementation spares its clients from the unspecified, generally chaotic ordering provided by `HashMap` (and `Hashtable`), without incurring the increased cost associated with `TreeMap`. It can be used to produce a copy of a map that has the same order as the original, regardless of the original map's implementation:

```
void foo(Map m) {
         Map copy = new LinkedHashMap(m);
         ...
     }
```

This technique is particularly useful if a module takes a map on input, copies it, and later returns results whose order is determined by that of the copy. (Clients generally appreciate having things returned in the same order they were presented.)

A special [constructor](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html#LinkedHashMap-int-float-boolean-) is provided to create a linked hash map whose order of iteration is the order in which its entries were last accessed, from least-recently accessed to most-recently (*access-order*). This kind of map is well-suited to building LRU caches. Invoking the `put`, `putIfAbsent`, `get`, `getOrDefault`, `compute`, `computeIfAbsent`, `computeIfPresent`, or `merge` methods results in an access to the corresponding entry (assuming it exists after the invocation completes). The `replace` methods only result in an access of the entry if the value is replaced. The `putAll` method generates one entry access for each mapping in the specified map, in the order that key-value mappings are provided by the specified map's entry set iterator. *No other methods generate entry accesses*. In particular, operations on collection-views do *not* affect the order of iteration of the backing map.

This class provides all of the optional `Map` operations, and permits `null` elements. Like `HashMap`, it provides constant-time performance for the basic operations (`add`, `contains` and `remove`), assuming the hash function disperses elements properly among the buckets. Performance is likely to be just slightly below that of `HashMap`, due to the added expense of maintaining the linked list, with one exception: Iteration over the collection-views of a `LinkedHashMap` requires time proportional to the *size* of the map, regardless of its capacity. Iteration over a `HashMap` is likely to be more expensive, requiring time proportional to its *capacity*.

A linked hash map has two parameters that affect its performance: *initial capacity* and *load factor*. They are defined precisely as for `HashMap`. Note, however, that the penalty for choosing an excessively high value for initial capacity is less severe for this class than for `HashMap`, as iteration times for this class are unaffected by capacity.

## Synchronization
Note that this implementation is not synchronized.  If multiple threads access a linked hash map concurrently, and at least one of the threads modifies the map structurally, it *must* be synchronized externally. This is typically accomplished by synchronizing on some object that naturally encapsulates the map. If no such object exists, the map should be "wrapped" using the `Collections.synchronizedMap` method. This is best done at creation time, to prevent accidental unsynchronized access to the map:

```
Map m = Collections.synchronizedMap(new LinkedHashMap(...));
```

A structural modification is any operation that adds or deletes one or more mappings or, in the case of access-ordered linked hash maps, affects iteration order. In insertion-ordered linked hash maps, merely changing the value associated with a key that is already contained in the map is not a structural modification. **In access-ordered linked hash maps, merely querying the map with get is a structural modification.** )

The iterators returned by the `iterator` method of the collections returned by all of this class's collection view methods are *fail-fast*: if the map is structurally modified at any time after the iterator is created, in any way except through the iterator's own remove method, the iterator will throw a `ConcurrentModificationException`. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw `ConcurrentModificationException` on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: *the fail-fast behavior of iterators should be used only to detect bugs*.

The spliterators returned by the spliterator method of the collections returned by all of this class's collection view methods are `late-binding`, `fail-fast`, and additionally report `Spliterator.ORDERED`.

## [LinkedHashMap vs HashMap](https://www.baeldung.com/java-linked-hashmap#linkedhashmap-vs-hashmap)
The `LinkedHashMap` class is very similar to `HashMap` in most aspects. However, the linked hash map is based on both hash table and linked list to enhance the functionality of hash map.

It maintains a doubly-linked list running through all its entries in addition to an underlying array of default size 16.

To maintain the order of elements, the linked hashmap modifies the `Map.Entry` class of `HashMap` by adding pointers to the next and previous entries:
```
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```

Notice that the `Entry` class simply adds two pointers; *before* and *after* which enable it to hook itself to the linked list. Aside from that, it uses the `Entry` class implementation of a the `HashMap`.

Finally, remember that this linked list defines the order of iteration, which by default is the order of insertion of elements (insertion-order).

## Example of usage
```
import java.util.LinkedHashMap;
import java.util.Map;

public final class LinkedHashMapExample {

    public void example() {
        Map<String, Integer> productsBasket = new LinkedHashMap<>();
        String product1 = "Product 1";
        String product2 = "Product 2";
        String product3 = "Product 3";
        String product4 = "Product 4";
        String product5 = "Product 5";

        productsBasket.put(product1, 10);
        productsBasket.put(product2, 20);
        productsBasket.put(product3, 30);
        productsBasket.put(product4, 40);
        productsBasket.put(product5, 50);

        String[] keyArray = productsBasket.keySet().toArray(new String[0]);

        for (String value : keyArray) {
            System.out.println("Key = " + value + ", value = " + productsBasket.get(value));
        }
    }
}
```

Output:
```
Key = Product 1, value = 10
Key = Product 2, value = 20
Key = Product 3, value = 30
Key = Product 4, value = 40
Key = Product 5, value = 50
```
We can guarantee that the insertion order will always be maintained. **We cannot make the same guarantee for a HashMap**.

## Conclusion
`LinkedHashMap` are:
- Extends `HashMap` class;
- Implements `Serializable`, `Clonable` and `Map` interfaces;
- Allowed `null` elements;
- It is an ordered collection which is the order in which elements were inserted into the map (*insertion-order*);
- Not synchronized;
- Time complexity average - `O(1)`;
- Time complexity worse - `O(n)`.

# Links
https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html  
https://www.baeldung.com/java-linked-hashmap

# Next questions
[How HashMap works?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/How%20HashMap%20works.md)  
[What do you know about ConcurrentHashMap?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20ConcurrentHashMap.md)   
[What do you know about EnumMap?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20EnumMap.md)  
[What do you know about TreeMap?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20TreeMap.md)

# Further reading
[LinkedHashMap in Java](https://www.geeksforgeeks.org/linkedhashmap-class-java-examples/)  
