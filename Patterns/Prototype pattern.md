# Prototype pattern
The Prototype Design Pattern is a creational pattern that enables the creation of new objects by copying an existing object. Prototype allows us to hide the complexity of making new instances from the client. The existing object acts as a prototype and contains the state of the object.<sup>[1](https://www.geeksforgeeks.org/prototype-design-pattern/#:~:text=03%20Jan%2C%202025-,The%20Prototype%20Design%20Pattern%20is%20a%20creational%20pattern%20that%20enables%20the%20creation%20of%20new%20objects%20by%20copying%20an%20existing%20object.%20Prototype%20allows%20us%20to%20hide%20the%20complexity%20of%20making%20new%20instances%20from%20the%20client.%20The%20existing%20object%20acts%20as%20a%20prototype%20and%20contains%20the%20state%20of%20the%20object.,-Table%20of%20Content)</sup>

The prototype design pattern solves problems like:<sup>[2](https://en.wikipedia.org/wiki/Prototype_pattern#:~:text=The%20prototype%20design%20pattern%20solves,dynamically%20loaded%20classes%20be%20instantiated%3F)</sup>
- How can objects be created so that the specific type of object can be determined at runtime?
- How can dynamically loaded classes be instantiated?

Creating objects directly within the class that requires (uses) the objects is inflexible because it commits the class to particular objects at compile-time and makes it impossible to specify which objects to create at run-time.<sup>[3](https://en.wikipedia.org/wiki/Prototype_pattern#:~:text=Creating%20objects%20directly%20within%20the%20class%20that%20requires%20(uses)%20the%20objects%20is%20inflexible%20because%20it%20commits%20the%20class%20to%20particular%20objects%20at%20compile%2Dtime%20and%20makes%20it%20impossible%20to%20specify%20which%20objects%20to%20create%20at%20run%2Dtime.)</sup>

The prototype design pattern describes how to solve such problems:<sup>[4](https://en.wikipedia.org/wiki/Prototype_pattern#:~:text=Creating%20objects%20directly%20within%20the%20class%20that%20requires%20(uses)%20the%20objects%20is%20inflexible%20because%20it%20commits%20the%20class%20to%20particular%20objects%20at%20compile%2Dtime%20and%20makes%20it%20impossible%20to%20specify%20which%20objects%20to%20create%20at%20run%2Dtime.)</sup>
- Define a `Prototype` object that returns a copy of itself;
- Create new objects by copying a `Prototype` object.

When to Use the Prototype Pattern?<sup>[5](https://java-design-patterns.com/patterns/prototype/#when-to-use-the-prototype-pattern-in-java)</sup>
- When the classes to instantiate are specified at run-time, for example, by dynamic loading;
- To avoid building a class hierarchy of factories that parallels the class hierarchy of products;
- When instances of a class can have one of only a few different combinations of state. It may be more convenient to install a corresponding number of prototypes and clone them rather than instantiating the class manually, each time with the appropriate state;
- When object creation is expensive compared to cloning;
- When the concrete classes to instantiate are unknown until runtime.

Real-World Use Cases:<sup>[6](https://www.javaguides.net/2023/10/prototype-design-pattern-in-kotlin.html#google_vignette:~:text=Real%2DWorld%20Use%20Cases)</sup>
- Spawning new game characters with similar attributes;
- Creating consistent UI components in design software;
- Cloning data structures for parallel processing.

## [Example](https://www.javaguides.net/2023/10/prototype-design-pattern-in-kotlin.html#google_vignette:~:text=6.-,Implementation%20in%20Kotlin%20Programming,-//%20Prototype%20interface%0Ainterface)
Implementation Steps:<sup>[7](https://www.javaguides.net/2023/10/prototype-design-pattern-in-kotlin.html#google_vignette:~:text=5.%20Implementation%20Steps,produce%20new%20objects.)</sup>
- Create a prototype interface that declares a cloning method;
- Implement the prototype in concrete classes;
- Use the prototype to clone and produce new objects.

```
// Prototype interface
interface GameCharacterPrototype {
    fun clone(): GameCharacterPrototype
}
// Concrete class implementing the Prototype
data class GameCharacter(val name: String, val weapon: String, val level: Int) : GameCharacterPrototype {
    override fun clone(): GameCharacterPrototype {
        return copy()
    }
}
fun main() {
    // Original character
    val originalCharacter = GameCharacter("Knight", "Sword", 5)
    // Cloning the character
    val clonedCharacter = originalCharacter.clone() as GameCharacter
    println("Original Character: $originalCharacter")
    println("Cloned Character: $clonedCharacter")
}
```

Output:
```
Original Character: GameCharacter(name=Knight, weapon=Sword, level=5)
Cloned Character: GameCharacter(name=Knight, weapon=Sword, level=5)
```

Explanation:<sup>[8](https://www.javaguides.net/2023/10/prototype-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,is%20cloned%20using%20the%20clone%20method.)</sup>
- `GameCharacterPrototype` is the prototype interface that declares a `clone` method;
- `GameCharacter` is a concrete class implementing this interface. The data class in Kotlin has a built-in copy method that fits perfectly with the prototype pattern, making cloning easier;
- In the main function, an original game character is created. Later, the character is cloned using the `clone` method.

## [Pros and Cons](https://java-design-patterns.com/patterns/prototype/#benefits-and-trade-offs-of-prototype-pattern)
Pros:
- Hides the complexities of instantiating new objects;
- Reduces the number of classes;
- Allows adding and removing objects at runtime.

Cons:
- Requires implementing a cloning mechanism which might be complex;
- Deep cloning can be difficult to implement correctly, especially if the classes have complex object graphs with circular references.

# Links
[Prototype Design Pattern](https://www.geeksforgeeks.org/prototype-design-pattern/)

[Prototype pattern](https://en.wikipedia.org/wiki/Prototype_pattern)

[Prototype Pattern in Java: Mastering Object Cloning for Efficient Instantiation](https://java-design-patterns.com/patterns/prototype/)

[Prototype Design Pattern in Kotlin](https://www.javaguides.net/2023/10/prototype-design-pattern-in-kotlin.html#google_vignette)

# Further reading
[Prototype Design Pattern](https://sourcemaking.com/design_patterns/prototype)

[Prototype](https://refactoring.guru/design-patterns/prototype)

[Prototype Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/prototype-software-pattern-kotlin-example)
