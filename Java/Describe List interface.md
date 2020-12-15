# List interface
![](./res/java_list_class_diagram.png "List class diagram")

A `List` is an ordered `Collection` (sometimes called a *sequence*). Lists may contain duplicate elements. In addition to the operations inherited from `Collection`, the `List` interface includes operations for the following:

- *Positional access* — manipulates elements based on their numerical position in the list. This includes methods such as `get`, `set`, `add`, `addAll`, and `remove`.
- *Search* -  searches for a specified object in the list and returns its numerical position. Search methods include `indexOf` and `lastIndexOf`.
- *Iteration* — extends *Iterator* semantics to take advantage of the list's sequential nature. The *listIterator* methods provide this behavior.
- *Range-view* — The `sublist` method performs arbitrary *range* operations on the list.

The Java platform contains two general-purpose `List` implementations. `ArrayList`, which is usually the better-performing implementation, and `LinkedList` which offers better performance under certain circumstances.

Like the `Set` interface, `List` strengthens the requirements on the `equals` and `hashCode` methods so that two `List` objects can be compared for logical equality without regard to their implementation classes. Two `List` objects are equal if they contain the same elements in the same order.

## Most used Implementing Classes

- **ArrayList**. Resizable-array implementation of the `List` interface. Implements all optional list operations, and permits all elements, including `null`. In addition to implementing the `List` interface, this class provides methods to manipulate the size of the array that is used internally to store the list. (This class is roughly equivalent to `Vector`, except that it is unsynchronized.)
- **LinkedList**. Doubly-linked list implementation of the `List` and `Deque` interfaces. Implements all optional list operations, and permits all elements (including `null`). All of the operations perform as could be expected for a doubly-linked list. Operations that index into the list will traverse the list from the beginning or the end, whichever is closer to the specified index.
- **Vector**.  The `Vector` class implements a growable array of objects. Like an array, it contains components that can be accessed using an integer index. However, the size of a `Vector` can grow or shrink as needed to accommodate adding and removing items after the `Vector` has been created. Vectors basically fall in legacy classes but now it is fully compatible with collections.
- **Stack**.  The `Stack` class represents a last-in-first-out (LIFO) stack of objects. It extends class `Vector` with five operations that allow a vector to be treated as a stack. The usual push and pop operations are provided, as well as a method to peek at the top item on the stack, a method to test for whether the stack is empty, and a method to search the stack for an item and discover how far it is from the top. When a stack is first created, it contains no items.

## Iterators
`Iterator` returned by List's iterator operation returns the elements of the list in proper sequence. `List` also provides a richer iterator, called a `ListIterator`, which allows you to traverse the list in either direction, modify the list during iteration, and obtain the current position of the iterator.

The three methods that `ListIterator` inherits from `Iterator` (`hasNext`, `next`, and `remove`) do exactly the same thing in both interfaces. The `hasPrevious` and the `previous` operations are exact analogues of `hasNext` and `next`. The former operations refer to the element before the (implicit) cursor, whereas the latter refer to the element after the cursor. The `previous` operation moves the cursor backward, whereas `next` moves it forward.

Here's the standard idiom for iterating backward through a list:
```
for (ListIterator<Type> it = list.listIterator(list.size()); it.hasPrevious(); ) {
    Type t = it.previous();
    ...
}
```

Note the argument to listIterator in the preceding idiom. The `List` interface has two forms of the listIterator method. The form with no arguments returns a `ListIterator` positioned at the beginning of the list; the form with an `int` argument returns a `ListIterator` positioned at the specified index. The index refers to the element that would be returned by an initial call to `next`. An initial call to `previous` would return the element whose index was `index-1`. In a list of length `n`, there are `n+1` valid values for `index`, from `0` to `n`, inclusive.

## Conclusion
`List` are:
- Member of the Java Collections Framework;
- Allows you to add duplicate elements;
- Allows you to have `null` elements;
- Indexes start from 0, just like arrays;
- Supports Generics;

# Links
https://docs.oracle.com/javase/tutorial/collections/interfaces/list.html  
https://docs.oracle.com/javase/8/docs/api/java/util/List.html  
https://www.journaldev.com/11444/java-list  

# Next questions
[What you know about ArrayList](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20ArrayList.md)  
[What you know about LinkedList](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20LinkedList.md)  
[What you know about Vector](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20Vector.md)  
[What you know about Stack](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20Stack.md)  
[ArrayList vs LinkedList](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/ArrayList%20vs%20LinkedList.md)  
