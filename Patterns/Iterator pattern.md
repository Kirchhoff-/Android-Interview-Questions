# Iterator pattern
The Iterator design pattern is a behavioral design pattern that provides a way to access the elements of an aggregate object (like a list) sequentially without exposing its underlying representation. It defines a separate object, called an iterator, which encapsulates the details of traversing the elements of the aggregate, allowing the aggregate to change its internal structure without affecting the way its elements are accessed.<sup>[1](https://www.geeksforgeeks.org/iterator-pattern/#:~:text=The%20Iterator%20design%20pattern%20is,way%20its%20elements%20are%20accessed.)</sup>

What problems can the Iterator design pattern solve?<sup>[2](https://en.wikipedia.org/wiki/Iterator_pattern#:~:text=What%20problems%20can%20the%20Iterator%20design%20pattern%20solve%3F%5B,be%20defined%20for%20an%20aggregate%20object%20without%20changing%20its%20interface.)</sup>
- The elements of an aggregate object should be accessed and traversed without exposing its representation (data structures);
- New traversal operations should be defined for an aggregate object without changing its interface.

Defining access and traversal operations in the aggregate interface is inflexible because it commits the aggregate to particular access and traversal operations and makes it impossible to add new operations later without having to change the aggregate interface.<sup>[3](https://en.wikipedia.org/wiki/Iterator_pattern#:~:text=Defining%20access%20and%20traversal%20operations%20in%20the%20aggregate%20interface%20is%20inflexible%20because%20it%20commits%20the%20aggregate%20to%20particular%20access%20and%20traversal%20operations%20and%20makes%20it%20impossible%20to%20add%20new%20operations%20later%20without%20having%20to%20change%20the%20aggregate%20interface.)</sup>

What solution does the Iterator design pattern describe?<sup>[4](https://en.wikipedia.org/wiki/Iterator_pattern#:~:text=What%20solution%20does%20the%20Iterator%20design%20pattern%20describe%3F%5B,and%20traverse%20an%20aggregate%20without%20knowing%20its%20representation%20(data%20structures).)</sup>
- Define a separate (iterator) object that encapsulates accessing and traversing an aggregate object;
- Clients use an iterator to access and traverse an aggregate without knowing its representation (data structures).

Different iterators can be used to access and traverse an aggregate in different ways. New access and traversal operations can be defined independently by defining new iterators.<sup>[5](https://en.wikipedia.org/wiki/Iterator_pattern#:~:text=Different%20iterators%20can%20be%20used%20to%20access%20and%20traverse%20an%20aggregate%20in%20different%20ways.%0ANew%20access%20and%20traversal%20operations%20can%20be%20defined%20independently%20by%20defining%20new%20iterators.)</sup>

## When to use
The Iterator design pattern is particularly useful in scenarios where:<sup>[6](https://medium.com/@mehar.chand.cloud/iterator-design-pattern-use-case-traverse-different-collections-bca646860c20#:~:text=The%20Iterator%20design%20pattern%20is,promoting%20code%20reuse%20and%20maintainability.)</sup>
- **Collections need to be traversed in a controlled manner**. When iterating over a collection’s elements in a specific order or with predefined conditions, the iterator pattern provides a structured and consistent way to manage the traversal;
- **Collection’s implementation details are hidden**. When the internal representation of a collection is complex or may change, the iterator pattern abstracts away the implementation details and provides a uniform interface for iteration;
- **Different collections need to be traversed similarly**. When dealing with various types of collections, the iterator pattern allows for consistent iteration across different implementations, promoting code reuse and maintainability.

## [Example](https://www.javaguides.net/2023/10/iterator-design-pattern-in-kotlin.html#google_vignette:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[7](https://www.javaguides.net/2023/10/iterator-design-pattern-in-kotlin.html#google_vignette:~:text=5.%20Implementation%20Steps,return%20its%20iterator.)</sup>
- Create an `Iterator` interface that defines methods like `hasNext()` and `next()`;
- Create a concrete iterator for each collection class;
- The collection class should have a method to return its iterator.

```
// Step 1: Define the Iterator interface
interface Iterator<T> {
    fun hasNext(): Boolean
    fun next(): T?
}
// Step 2: Concrete Iterator
class BookShelfIterator(private val bookShelf: BookShelf) : Iterator<Book> {
    private var index = 0
    override fun hasNext(): Boolean {
        return index < bookShelf.getLength()
    }
    override fun next(): Book? {
        return bookShelf.getBookAt(index++)
    }
}
// Aggregate class
class BookShelf : Iterable<Book> {
    private val books = mutableListOf<Book>()
    fun addBook(book: Book) {
        books.add(book)
    }
    fun getLength() = books.size
    fun getBookAt(index: Int) = books[index]
    override fun iterator(): Iterator<Book> = BookShelfIterator(this)
}
data class Book(val name: String)
// Client Code
fun main() {
    val bookShelf = BookShelf()
    bookShelf.addBook(Book("Design Patterns"))
    bookShelf.addBook(Book("Effective Java"))
    bookShelf.addBook(Book("Clean Code"))
    val iterator = bookShelf.iterator()
    while (iterator.hasNext()) {
        val book = iterator.next()
        println(book?.name)
    }
}
```

Output:
```
Design Patterns
Effective Java
Clean Code
```

Explanation:<sup>[8](https://www.javaguides.net/2023/10/iterator-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,using%20the%20BookShelfIterator.)</sup>
- We defined an `Iterator` interface with essential methods: `hasNext()` and `next()`;
- `BookShelfIterator` is a concrete implementation of this interface, tailored to work with the `BookShelf` class;
- The `BookShelf` class, representing our collection, implements the `Iterable` interface and returns its custom iterator;
- The client code demonstrates how to iterate over the `BookShelf` collection using the `BookShelfIterator`.

## Pros and Cons
Advantages:<sup>[9](https://medium.com/system-design-by-harsh-khandelwal/iterator-design-pattern-a-comprehensive-guide-e8711ff48329#:~:text=Advantages%3A,Single%20Responsibility%20Principle.)</sup>
- **Simplifies Collection Traversal**. It provides a standard way to iterate through different types of collections;
- **Decouples Algorithms from Collections**. You can use the same iteration logic with different types of collections;
- **Supports Multiple Traversals**. You can have multiple iterators on the same collection simultaneously;
- **Promotes Single Responsibility**. The iteration logic is separated from the collection, adhering to the Single Responsibility Principle.

Disadvantages:<sup>[10](https://medium.com/system-design-by-harsh-khandelwal/iterator-design-pattern-a-comprehensive-guide-e8711ff48329#:~:text=Disadvantages%3A,accessing%20elements%20directly.)</sup>
- **Increased Complexity**. For simple collections, using an iterator might be overkill;
- **Performance Overhead**. In some cases, using an iterator might be less efficient than accessing elements directly.

# Links
[Iterator Design Pattern](https://www.geeksforgeeks.org/iterator-pattern/)

[Iterator pattern](https://en.wikipedia.org/wiki/Iterator_pattern)

[Iterator Design Pattern Use Case: Traverse Different Collections](https://medium.com/@mehar.chand.cloud/iterator-design-pattern-use-case-traverse-different-collections-bca646860c20)

[Iterator Design Pattern in Kotlin](https://www.javaguides.net/2023/10/iterator-design-pattern-in-kotlin.html#google_vignette)

[Iterator Design Pattern: A Comprehensive Guide](https://medium.com/system-design-by-harsh-khandelwal/iterator-design-pattern-a-comprehensive-guide-e8711ff48329)

# Further reading
[Iterator Design Pattern](https://sourcemaking.com/design_patterns/memento)

[Iterator](https://refactoring.guru/design-patterns/iterator)

[Iterator Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/iterator-software-pattern-kotlin-example)

[The Iterator Design Pattern in Kotlin: Simplified and Explained](https://medium.com/softaai-blogs/the-iterator-design-pattern-in-kotlin-simplified-and-explained-aac3d174a7aa)
