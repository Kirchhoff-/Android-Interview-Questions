# Memento pattern
The Memento design pattern is a powerful behavioral pattern that provides a way to capture and restore an object’s internal state. It allows objects to be saved and restored without violating encapsulation principles.<sup>[1](https://neatcode.org/memento-pattern/#:~:text=The%20Memento%20design%20pattern%20is%20a%20powerful%20behavioral%20pattern%20that%20provides%20a%20way%20to%20capture%20and%20restore%20an%20object%E2%80%99s%20internal%20state.%20It%20allows%20objects%20to%20be%20saved%20and%20restored%20without%20violating%20encapsulation%20principles.)</sup>

The pattern consists of three main components: the **Originator**, the **Memento**, and the **Caretaker**:<sup>[2](https://neatcode.org/memento-pattern/#:~:text=The%20pattern%20consists,using%20Memento%20objects.)</sup>
- **Originator**. The Originator is the object whose state we want to capture and restore. It creates a Memento object containing a snapshot of its current state or can restore its state from a Memento object;
- **Memento**. The Memento is an object that stores the state of the Originator. It provides methods to retrieve the saved state and potentially modify it;
- **Caretaker**. The Caretaker is responsible for storing and managing the Memento objects. It interacts with the Originator to save and restore its state using Memento objects.

When to use Memento Design Pattern:<sup>[3](https://www.geeksforgeeks.org/memento-design-pattern/#:~:text=content%0AMore%20content-,When%20to%20use%20Memento%20Design%20Pattern%3F,reduce%20duplicate%20calculations%20or%20enhance%20efficiency%20by%20caching%20an%20object%27s%20state.,-When%20not%20to)</sup>
- **Undo functionality**. When your application needs to include an undo function that lets users restore the state of an object after making modifications;
- **Snapshotting**. When you need to enable features like versioning or checkpoints by saving an object's state at different times;
- **Transaction rollback**. When there are failures or exceptions, like in database transactions, and you need to reverse changes made to an object's state;
- **Caching**. When you wish to reduce duplicate calculations or enhance efficiency by caching an object's state.

## [Example](https://www.javaguides.net/2023/10/memento-design-pattern-in-kotlin.html#google_vignette:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[4](https://www.javaguides.net/2023/10/memento-design-pattern-in-kotlin.html#google_vignette:~:text=Implementation%20Steps,undo%20and%20redo.)</sup>
- Create the `Memento` class to store the internal state of the `Originator`;
- The `Originator` class should have methods to save its state to a memento and restore its state from a memento;
- Implement a `Caretaker` class that maintains a list of mementos for the purpose of undo and redo.

```
// Step 1: Memento class
data class Memento(val state: String)
// Step 2: Originator class
class Originator(var state: String) {
    fun createMemento(): Memento {
        return Memento(state)
    }
    fun restore(memento: Memento) {
        state = memento.state
    }
}
// Step 3: Caretaker class
class Caretaker {
    private val mementoList = mutableListOf<Memento>()
    fun saveState(originator: Originator) {
        mementoList.add(originator.createMemento())
    }
    fun undo(originator: Originator) {
        if (mementoList.isNotEmpty()) {
            val memento = mementoList.removeAt(mementoList.size - 1)
            originator.restore(memento)
        }
    }
    // More methods like redo can be added similarly
}
// Client Code
fun main() {
    val originator = Originator("Initial State")
    val caretaker = Caretaker()
    caretaker.saveState(originator)
    originator.state = "State #1"
    caretaker.saveState(originator)
    originator.state = "State #2"
    println("Current State: ${originator.state}")
    caretaker.undo(originator)
    println("After undo: ${originator.state}")
}
```

Output:
```
Current State: State #2
After undo: State #1
```

Explanation:<sup>[5](https://www.javaguides.net/2023/10/memento-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,undo%20and%20redo.)</sup>
- The `Memento` captures the internal state of the `Originator` without violating its encapsulation;
- The `Originator` can save and restore its state using the memento;
- The `Caretaker` provides a mechanism to keep track of multiple mementos, allowing for functionalities like undo and redo.

## [Pros and Cons](https://dev.to/diegosilva13/understanding-the-memento-design-pattern-in-java-2c72#:~:text=Pros%20and%20Cons,to%20the%20system.)
Pros:
- **Preserves Encapsulation**. Allows an object's internal state to be saved and restored without exposing its implementation details;
- **Simple Undo/Redo**. Facilitates the implementation of undo/redo functionality, making the system more robust and user-friendly;
- **State History**. Allows maintaining a history of previous states of the object, enabling navigation between different states.

Cons:
- **Memory Consumption**. Storing multiple mementos can consume significant memory, especially if the object's state is large;
- **Additional Complexity**. Introduces additional complexity to the code, with the need to manage the creation and restoration of mementos;
- **Caretaker Responsibility**. The caretaker needs to manage mementos efficiently, which can add responsibility and complexity to the system.

# Links
[Memento Design Pattern – Implement Undo/Redo Functionality](https://neatcode.org/memento-pattern/)

[Memento Design Pattern](https://www.geeksforgeeks.org/memento-design-pattern/)

[Memento Design Pattern in Kotlin](https://www.javaguides.net/2023/10/memento-design-pattern-in-kotlin.html#google_vignette)

[Understanding the Memento Design Pattern in Java](https://dev.to/diegosilva13/understanding-the-memento-design-pattern-in-java-2c72)

# Further reading
[Memento Design Pattern](https://sourcemaking.com/design_patterns/memento)

[Memento](https://refactoring.guru/design-patterns/memento)

[Memento Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/memento-software-pattern-kotlin-example)
