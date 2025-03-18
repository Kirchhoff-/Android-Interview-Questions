# Mediator pattern
Mediator is a behavioral design pattern that reduces coupling between components of a program by making them communicate indirectly, through a special mediator object.

The Mediator makes it easy to modify, extend and reuse individual components because they’re no longer dependent on the dozens of other classes.<sup>[1](https://refactoring.guru/design-patterns/mediator/java/example#:~:text=Mediator%20is%20a,of%20other%20classes.)</sup>

Problems that the mediator design pattern can solve:
- Tight coupling between a set of interacting objects should be avoided;
- It should be possible to change the interaction between a set of objects independently.

Defining a set of interacting objects by accessing and updating each other directly is inflexible because it tightly couples the objects to each other and makes it impossible to change the interaction independently from (without having to change) the objects. And it stops the objects from being reusable and makes them hard to test.

Tightly coupled objects are hard to implement, change, test, and reuse because they refer to and know about many different objects.<sup>[2](https://en.wikipedia.org/wiki/Mediator_pattern#:~:text=Problems%20that%20the%20mediator%20design%20pattern%20can%20solve%5B,they%20refer%20to%20and%20know%20about%20many%20different%20objects.)</sup>

Solutions described by the mediator design pattern:
- Define a separate (mediator) object that encapsulates the interaction between a set of objects;
- Objects delegate their interaction to a mediator object instead of interacting with each other directly.

The objects interact with each other indirectly through a mediator object that controls and coordinates the interaction. This makes the objects loosely coupled. They only refer to and know about their mediator object and have no explicit knowledge of each other.<sup>[3](https://en.wikipedia.org/wiki/Mediator_pattern#:~:text=Solutions%20described%20by%20the%20mediator%20design%20pattern%5B,and%20have%20no%20explicit%20knowledge%20of%20each%20other.)</sup>

## [Real world example of mediator pattern](https://howtodoinjava.com/design-patterns/behavioral/mediator-pattern/#2-real-world-example-of-mediator-pattern)
- A great real world example of mediator pattern is **traffic control room** at airports. If all flights will have to interact with each other for finding which flight is going to land next, it will create a big mess. Rather flights only send their status to the tower. These towers in turn send the signals to conform which airplane can take-off or land. We must note that these towers do not control the whole flight. They only enforce constraints in the terminal areas;
- Another good example of mediator pattern is a **chat application**. In a chat application we can have several participants. It’s not a good idea to connect each participant to all the others because the number of connections would be really high. The best solution is to have a hub where all participants will connect. This hub is just the mediator class.

## [Example](https://www.javaguides.net/2023/10/mediator-design-pattern-in-kotlin.html#:~:text=Implementation%20in%20Kotlin%20Programming)
```
// Step 1: Define the Mediator interface
interface Mediator {
    fun notify(sender: Colleague, event: String)
}
// Step 2: Create concrete mediator class
class ConcreteMediator : Mediator {
    lateinit var colleague1: ColleagueA
    lateinit var colleague2: ColleagueB
    override fun notify(sender: Colleague, event: String) {
        when (event) {
            "A" -> colleague2.reactToA()
            "B" -> colleague1.reactToB()
        }
    }
}
// Base colleague class
abstract class Colleague(protected val mediator: Mediator)
class ColleagueA(mediator: Mediator) : Colleague(mediator) {
    fun doA() {
        println("ColleagueA triggers event A.")
        mediator.notify(this, "A")
    }
    fun reactToB() {
        println("ColleagueA reacts to event B.")
    }
}
class ColleagueB(mediator: Mediator) : Colleague(mediator) {
    fun doB() {
        println("ColleagueB triggers event B.")
        mediator.notify(this, "B")
    }
    fun reactToA() {
        println("ColleagueB reacts to event A.")
    }
}
// Client Code
fun main() {
    val mediator = ConcreteMediator()
    val colleagueA = ColleagueA(mediator)
    val colleagueB = ColleagueB(mediator)
    mediator.colleague1 = colleagueA
    mediator.colleague2 = colleagueB
    colleagueA.doA()
    colleagueB.doB()
}
```

Output:
```
ColleagueA triggers event A.
ColleagueB reacts to event A.
ColleagueB triggers event B.
ColleagueA reacts to event B.
```

- A `Mediator` interface defines the contract for communication. The `ConcreteMediator` class implements this interface;
- `Colleague` classes, `ColleagueA` and `ColleagueB`, represent components that interact with each other via the mediator;
- Instead of directly triggering reactions in the opposite colleague, each colleague reports to the mediator, which handles the logic and coordination.<sup>[4](https://www.javaguides.net/2023/10/mediator-design-pattern-in-kotlin.html#:~:text=1.%20A%20Mediator,logic%20and%20coordination.)</sup>

## [When to use the Mediator Design Pattern?](https://www.geeksforgeeks.org/mediator-design-pattern/#:~:text=landing%20clearance.-,When%20to%20use%20the%20Mediator%20Design%20Pattern%3F,-Complex%20Communication%3A)
- **Complex Communication**. Your system involves a set of objects that need to communicate with each other in a complex manner, and you want to avoid direct dependencies between them;
- **Loose Coupling**. You want to promote loose coupling between objects, allowing them to interact without knowing the details of each other’s implementations;
- **Centralized Control**. You need a centralized mechanism to coordinate and control the interactions between objects, ensuring a more organized and maintainable system;
- **Changes in Behavior**. You anticipate changes in the behavior of components, and you want to encapsulate these changes within the mediator, preventing widespread modifications;
- **Enhanced Reusability**. You want to reuse individual components in different contexts without altering their internal logic or communication patterns.

## [Cons of the Mediator Pattern](https://www.baeldung.com/kotlin/mediator#2-cons-of-the-mediator-pattern)
While the mediator pattern offers various advantages in terms of decoupling and centralized communication management, it’s important to acknowledge its potential disadvantages:
- Introducing a mediator can sometimes lead to an increase in overall system complexity. As the number of mediators and their responsibilities grow, managing and understanding the interactions between components may become more challenging;
- The mediator becomes a central point for communication. If the mediator fails or experiences issues, the entire system’s communication may be jeopardized, potentially leading to a single point of failure;
- Maintaining a clean and concise mediator interface becomes crucial as the system evolves. Adding new functionalities or modifying existing ones in the mediator might require adjustments in multiple components, making the system more challenging to maintain.

# Links
[Mediator in Java](https://refactoring.guru/design-patterns/mediator/java/example)

[Mediator pattern](https://en.wikipedia.org/wiki/Mediator_pattern)

[Mediator Design Pattern](https://howtodoinjava.com/design-patterns/behavioral/mediator-pattern/)

[Mediator Design Pattern in Kotlin](https://www.javaguides.net/2023/10/mediator-design-pattern-in-kotlin.html)

[Mediator Design Pattern](https://www.geeksforgeeks.org/mediator-design-pattern/)

[Mediator Pattern in Kotlin](https://www.baeldung.com/kotlin/mediator)

# Further reading
[Mediator](https://refactoring.guru/design-patterns/mediator)

[Mediator in Kotlin](https://swiderski.tech/kotlin_mediator_pattern/)

[Mediator Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/mediator-software-pattern-kotlin-example)

[Mediator Design Pattern in Java](https://medium.com/@kalanamalshan98/mediator-pattern-in-java-755662d33c0f)

# Next questions
[What is coupling?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20is%20coupling.md)
