# Vector
The `Vector` class implements a growable array of objects. Like an array, it contains components that can be accessed using an integer index. However, the size of a `Vector` can grow or shrink as needed to accommodate adding and removing items after the `Vector` has been created. Vectors basically fall in legacy classes but now it is fully compatible with collections.

`Vector` defines three protected data member:

- `int capacityIncreament`: Contains the increment value.
- `int elementCount`: Number of elements currently in vector stored in it.
- `Object elementData[]`: Array that holds the vector is stored in it.

Each vector tries to optimize storage management by maintaining a `capacity` and a `capacityIncrement`. The `capacity` is always at least as large as the vector size; it is usually larger because as components are added to the vector, the vector's storage increases in chunks the size of `capacityIncrement`. An application can increase the capacity of a vector before inserting a large number of components; this reduces the amount of incremental reallocation.

If a thread-safe implementation is not needed, it is recommended to use [ArrayList](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/vector_java/Java/What%20you%20know%20about%20ArrayList.md) in place of `Vector`. `Vector` proves to be very useful if you don't know the size of the array in advance or you just need one that can change sizes over the lifetime of a program.

## Synchronization
Unlike the new collection implementations, `Vector` is synchronized.

The iterators returned by this class's `iterator` and `listIterator` methods are *fail-fast*: if the vector is structurally modified at any time after the iterator is created, in any way except through the iterator's own `remove` or `add` methods, the iterator will throw a `ConcurrentModificationException`. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future. The `Enumerations` returned by the `elements` method are *not* fail-fast.

Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw `ConcurrentModificationException` on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: *the fail-fast behavior of iterators should be used only to detect bugs*.

## Example of usage
```
import java.util.*;

public class VectorDemo {

   public static void main(String args[]) {
        // Initial capacity(size) of 3
        Vector<String> vector = new Vector<>(3);

        vector.addElement("First");
        vector.addElement("Second");
        vector.addElement("Third");
        vector.addElement("Fourth");

        System.out.println("Size is: " + vector.size());
        System.out.println("Capacity increment is: " + vector.capacity());

        vector.addElement("Fifth");
        vector.addElement("Sixth");
        vector.addElement("Seventh");

        System.out.println("Size is: " + vector.size());
        System.out.println("Capacity increment is: " + vector.capacity());

        // Display Vector elements
        System.out.println("Elements are:");
        for (String str : vector) {
            System.out.println(str);
        }
   }
}
```

Output:
```
Size is: 4
Capacity increment is: 6
Size is: 7
Capacity increment is: 12
Elements are:
First
Second
Third
Fourth
Fifth
Sixth
Seventh
```

## Conclusion
`Vector` are:
- Inherits the `AbstractList` class;
- Implements `List` interface;
- Implements `Serializable`, `Cloneable`, `Iterable<E>`, `Collection<E>`, `List<E>`, `RandomAccess` interfaces;
- Synchronized;
- Perform `get` and `set` operation for constant time - `O(1)`;
- Perform `add` and `remove` operation for *amortized constant time* - `O(n)`.

# Links
https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html  
https://www.geeksforgeeks.org/java-util-vector-class-java/  
https://www.tutorialspoint.com/java/java_vector_class.htm  

# Next questions
[What you know about ArrayList?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20ArrayList.md)  
[What you know about LinkedList?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20LinkedList.md)
