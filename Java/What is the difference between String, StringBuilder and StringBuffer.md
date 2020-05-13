# String vs StringBuilder vs StringBuffer

`String` - class represents character strings. All string literals in Java programs, such as "abc", are implemented as instances of this class. `Strings` are constant; their values cannot be changed after they are created. String buffers support mutable strings. Because String objects are immutable they can be shared. The Java language provides special support for the string concatenation operator ( + ), and for conversion of other objects to strings. String concatenation is implemented through the `StringBuilder`(or `StringBuffer`) class and its append method. String conversions are implemented through the method `toString()`, defined by `Object` and inherited by all classes in Java

`StringBuffer` - A thread-safe, mutable sequence of characters. A string buffer is like a String, but can be modified. At any point in time it contains some particular sequence of characters, but the length and content of the sequence can be changed through certain method calls. String buffers are safe for use by multiple threads. The methods are synchronized where necessary so that all the operations on any particular instance behave as if they occur in some serial order that is consistent with the order of the method calls made by each of the individual threads involved. Whenever an operation occurs involving a source sequence (such as appending or inserting from a source sequence) this class synchronizes only on the string buffer performing the operation, not on the source.

`StringBuilder` - A mutable sequence of characters. This class provides an API compatible with StringBuffer, but with no guarantee of synchronization. This class is designed for use as a drop-in replacement for StringBuffer in places where the string buffer was being used by a single thread (as is generally the case). Where possible, it is recommended that this class be used in preference to StringBuffer as it will be faster under most implementations. The principal operations on a `StringBuilder` are the append and insert methods, which are overloaded so as to accept data of any type. Each effectively converts a given datum to a string and then appends or inserts the characters of that string to the string builder. The append method always adds these characters at the end of the builder; the insert method adds the characters at a specified point. 

## Conclusion
- The `String` object is immutable in Java but `StringBuffer` and `StringBuilder` are mutable objects.
- `StringBuffer` is synchronized while `StringBuilder` is not which makes `StringBuilder` faster than `StringBuffer`.
- Concatenation operator "+" is internally implemented using either `StringBuffer` or `StringBuilder`.
- Use `String` if you require immutability, use `StringBuffer` if you need mutable + thread-safety and use `StringBuilder` if you require mutable + without thread-safety.

## Links
https://javarevisited.blogspot.com/2011/07/string-vs-stringbuffer-vs-stringbuilder.html  
https://www.journaldev.com/538/string-vs-stringbuffer-vs-stringbuilder  
https://www.geeksforgeeks.org/string-vs-stringbuilder-vs-stringbuffer-in-java/  
https://docs.oracle.com/javase/7/docs/api/java/lang/String.html  
https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html  
https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html  
