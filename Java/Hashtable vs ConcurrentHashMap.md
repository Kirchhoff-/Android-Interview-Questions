# Hashtable vs ConcurrentHashMap

**Hashtable**. This class implements a hash table, which maps keys to values. Any non-null object can be used as a key or as a value. To successfully store and retrieve objects from a hashtable, the objects used as keys must implement the `hashCode` method and the `equals` method. Unlike the new collection implementations, `Hashtable` is synchronized. If a thread-safe implementation is not needed, it is recommended to use `HashMap` in place of `Hashtable`.

**ConcurrentHashMap**. A hash table supporting full concurrency of retrievals and high expected concurrency for updates. This class obeys the same functional specification as `Hashtable`, and includes versions of methods corresponding to each method of `Hashtable`. However, even though all operations are thread-safe, retrieval operations do *not* entail locking, and there is *not* any support for locking the entire table in a way that prevents all access. This class is fully interoperable with `Hashtable` in programs that rely on its thread safety but not on its synchronization details.

| Properties | `Hashtable` | `ConcurrentHashMap` |
|---|---|---|
| Is `null` key allowed   |  No  |  No  |
| Is `null` value allowed  |  No  |  Yes  |
| Is Thread Safe  | Yes | Yes, Thread safety is ensured by having separate locks for separate buckets, resulting in better performance. Performance is further improved by providing read access concurrently without any blocking.  |
| Performance  | Slow due to synchronization overhead.  | Faster than `Hashtable`. `ConcurrentHashMap` is a better choice when there are more reads than writes. |
| Iterator | `Hashtable` uses enumerator to iterate the values of `Hashtable` object. Enumerations returned by the `Hashtable` keys and elements methods are not fail-fast.  | Fail-safe iterator: `Iterator` provided by the `ConcurrentHashMap` is fail-safe, which means it will not throw `ConcurrentModificationException.`  |

When you read from a `ConcurrentHashMap` using `get()`, there are no locks, contrary to the `Hashtable` for which all operations are simply synchronized. `Hashtable` uses single lock for whole data. `ConcurrentHashMap` uses multiple locks on segment level (16 by default) instead of object level i.e. whole `Map`. `Hashtable` is belongs to the Collection framework; `ConcurrentHashMap` belongs to the Executor framework. If a thread-safe highly-concurrent implementation is desired, then it is recommended to use `ConcurrentHashMap` in place of `Hashtable`. 

# Links
https://www.geeksforgeeks.org/concurrenthashmap-in-java/  
https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html  
https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html

# Next questions
[Describe map interface from java collection](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/Describe%20map%20interface%20from%20java%20collection.md)  
[What do you know about Hashtable?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20Hashtable.md)  
[What do you know about ConcurrentHashMap?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20do%20you%20know%20about%20ConcurrentHashMap.md)
