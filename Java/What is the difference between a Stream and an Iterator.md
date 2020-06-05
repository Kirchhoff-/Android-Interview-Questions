# Stream vs Iterator

**Stream** represents a sequence of objects from a source, which supports aggregate operations. Stream doesn't store data and it's not a data structure. Also it never modifies data source from which it was created. Stream supports functional style operations (map, reduce and etc.) on streams of elements.

**Iterator** is interface which helps retrieve elements from collection one by one. To use a Java `Iterator` you will have to obtain an `Iterator` instance from the collection of objects you want to iterate over. The obtained `Iterator` keeps track of the elements in the underlying collection to make sure you iterate through all of them. If you modify the underlying collection while iterating through an `Iterator` pointing to that collection, the `Iterator` will typically detect it, and throw an exception the next time you try to obtain the next element from the `Iterator`.

Streams differ from collections in several ways:
- No storage. A stream is not a data structure that stores elements; instead, it conveys elements from a source such as a data structure, an array, a generator function, or an I/O channel, through a pipeline of computational operations.
- Functional in nature. An operation on a stream produces a result, but does not modify its source. For example, filtering a `Stream` obtained from a collection produces a new `Stream` without the filtered elements, rather than removing elements from the source collection.
- Laziness-seeking. Many stream operations, such as filtering, mapping, or duplicate removal, can be implemented lazily, exposing opportunities for optimization. For example, "find the first String with three consecutive vowels" need not examine all the input strings. Stream operations are divided into intermediate (Stream-producing) operations and terminal (value- or side-effect-producing) operations. Intermediate operations are always lazy.
- Possibly unbounded. While collections have a finite size, streams need not. Short-circuiting operations such as `limit(n)` or `findFirst()` can allow computations on infinite streams to complete in finite time.
- Consumable. The elements of a stream are only visited once during the life of a stream. Like an `Iterator`, a new stream must be generated to revisit the same elements of the source.

## Links
https://www.tutorialspoint.com/java8/java8_streams.htm  
http://tutorials.jenkov.com/java-collections/iterator.html  
https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#package.description  
