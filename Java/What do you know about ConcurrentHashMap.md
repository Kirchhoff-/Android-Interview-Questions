# ConcurrentHashMap

A hash table supporting full concurrency of retrievals and high expected concurrency for updates. This class obeys the same functional specification as [Hashtable](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20Hashtable.md), and includes versions of methods corresponding to each method of `Hashtable`. However, even though all operations are thread-safe, retrieval operations do *not* entail locking, and there is *not* any support for locking the entire table in a way that prevents all access. This class is fully interoperable with `Hashtable` in programs that rely on its thread safety but not on its synchronization details.

Retrieval operations (including `get`) generally do not block, so may overlap with update operations (including `put` and `remove`). Retrievals reflect the results of the most recently *completed* update operations holding upon their onset. (More formally, an update operation for a given key bears a *happens-before* relation with any (non-null) retrieval for that key reporting the updated value.) For aggregate operations such as `putAll` and `clear`, concurrent retrievals may reflect insertion or removal of only some entries. Similarly, Iterators, Spliterators and Enumerations return elements reflecting the state of the hash table at some point at or since the creation of the iterator/enumeration. They do *not* throw `ConcurrentModificationException`. However, iterators are designed to be used by only one thread at a time. Bear in mind that the results of aggregate status methods including `size`, `isEmpty`, and `containsValu`e are typically useful only when a map is not undergoing concurrent updates in other threads. Otherwise the results of these methods reflect transient states that may be adequate for monitoring or estimation purposes, but not for program control.

The table is dynamically expanded when there are too many collisions (i.e., keys that have distinct hash codes but fall into the same slot modulo the table size), with the expected average effect of maintaining roughly two bins per mapping (corresponding to a 0.75 load factor threshold for resizing). There may be much variance around this average as mappings are added and removed, but overall, this maintains a commonly accepted time/space tradeoff for hash tables. However, resizing this or any other kind of hash table may be a relatively slow operation. When possible, it is a good idea to provide a size estimate as an optional `initialCapacity` constructor argument. An additional optional `loadFactor` constructor argument provides a further means of customizing initial table capacity by specifying the table density to be used in calculating the amount of space to allocate for the given number of elements. Also, for compatibility with previous versions of this class, constructors may optionally specify an expected `concurrencyLevel` as an additional hint for internal sizing. Note that using many keys with exactly the same `hashCode()` is a sure way to slow down performance of any hash table. To ameliorate impact, when keys are `Comparable`, this class may use comparison order among keys to help break ties.

Like `Hashtable` but unlike `HashMap`, this class does not allow `null` to be used as a key or value.

Constructors of `ConcurrentHashMap`:
- **Concurrency-Level**: It is the number of threads concurrently updating the map. The implementation performs internal sizing to try to accommodate this many threads.
- **Load-Factor**: It’s a threshold, used to control resizing.
- **Initial Capacity**: Accommodation of a certain number of elements initially provided by the implementation. if the capacity of this map is 10. It means that it can store 10 entries.

## Conclusion
`ConcurrentHashMap` are:
- Extends `AbstractMap` class;
- Implements `ConcurrentMap` and `Serializable` interfaces;
- Doesn't allow `null` key or value;
- Thread-safe;

# Links
https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html  
https://www.geeksforgeeks.org/concurrenthashmap-in-java/  


# Next questions
[What do you know about Hashtable?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20Hashtable.md)  
[How HashMap works?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/How%20HashMap%20works.md)
