
  ## Collection(Kotlin)

`Collection` usually contains a number of objects of the same type. Objects in a collection are called `elements` or `items`. For example, all the students in a department form a collection that can be used to calculate their average age. The following collection types are relevant for Kotlin:

- **List** is an ordered collection with access to elements by indices – integer numbers that reflect their position. Elements can occur more than once in a list.

- **Set** is a collection of unique elements. It reflects the mathematical abstraction of set: a group of objects without repetitions.

- **Map (or dictionary)** is a set of key-value pairs. Keys are unique, and each of them maps to exactly one value. The values can be duplicates. Maps are useful for storing logical connections between objects.

  

### Collection Types

The Kotlin Standard Library provides implementations for basic collection types: sets, lists, and maps. A pair of interfaces represent each collection type:

  

- A **read-only** interface that provides operations for accessing collection elements.The read-only collection types are `covariant`( collection types have the same subtyping relationship as the element types. If a `Rectangle` class inherits from `Shape`, you can use a `List<Rectangle>` anywhere the `List<Shape>` is required.)

- A **mutable** interface that extends the corresponding read-only interface with write operations: adding, removing, and updating its elements.

The mutable collections aren't covariant; otherwise, this would lead to runtime failures. If `MutableList<Rectangle>` was a subtype of `MutableList<Shape>`, you could insert other `Shape` inheritors (for example, `Circle`) into it, thus violating its `Rectangle` type argument.

  

Below is a diagram of the Kotlin collection interfaces

  

![enter image description here](https://raw.githubusercontent.com/swayangjit/Android-Interview-Questions/master/Kotlin/res/collections-diagram.png)

  
## Links
 https://kotlinlang.org/docs/reference/collections-overview.html