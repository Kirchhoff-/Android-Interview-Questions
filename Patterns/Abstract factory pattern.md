# Abstract factory pattern
The Abstract Factory is a software design pattern whose goal is to provide a single interface to create families of objects with the same theme but without exposing the concrete implementation<sup>[1](https://www.baeldung.com/kotlin/abstract-factory-pattern#:~:text=The%20Abstract%20Factory%20is%20a%20software%20design%20pattern%20whose%20goal%20is%20to%20provide%20a%20single%20interface%20to%20create%20families%20of%20objects%20with%20the%20same%20theme%20but%20without%20exposing%20the%20concrete%20implementation.)</sup>. It may be used to solve problems such as<sup>[2](https://en.wikipedia.org/wiki/Abstract_factory_pattern#:~:text=It%20may%20be,objects%20be%20created%3F)</sup>:
- How can an application be independent of how its objects are created?
- How can a class be independent of how the objects that it requires are created?
- How can families of related or dependent objects be created?

Creating objects directly within the class that requires the objects is inflexible. Doing so commits the class to particular objects and makes it impossible to change the instantiation later without changing the class. It prevents the class from being reusable if other objects are required, and it makes the class difficult to test because real objects cannot be replaced with mock objects.

A factory is the location of a concrete class in the code at which objects are constructed. Implementation of the pattern intends to insulate the creation of objects from their usage and to create families of related objects without depending on their concrete classes. This allows for new derived types to be introduced with no change to the code that uses the base class.<sup>[3](https://en.wikipedia.org/wiki/Abstract_factory_pattern#:~:text=Creating%20objects%20directly,base%20class.)</sup>

The pattern describes how to solve such problems<sup>[4](https://en.wikipedia.org/wiki/Abstract_factory_pattern#:~:text=The%20pattern%20describes,creating%20objects%20directly.):
- Encapsulate object creation in a separate (factory) object by defining and implementing an interface for creating objects;
- Delegate object creation to a factory object instead of creating objects directly.

This makes a class independent of how its objects are created. A class may be configured with a factory object, which it uses to create objects, and the factory object can be exchanged at runtime.<sup>[5](https://en.wikipedia.org/wiki/Abstract_factory_pattern#:~:text=This%20makes%20a%20class%20independent%20of%20how%20its%20objects%20are%20created.%20A%20class%20may%20be%20configured%20with%20a%20factory%20object%2C%20which%20it%20uses%20to%20create%20objects%2C%20and%20the%20factory%20object%20can%20be%20exchanged%20at%20runtime.)

When to Use Abstract Factory Pattern<sup>[6](https://www.baeldung.com/java-abstract-factory-pattern#:~:text=When%20to%20Use,a%20particular%20dependency)</sup>?
- The client is independent of how we create and compose the objects in the system;
- The system consists of multiple families of objects, and these families are designed to be used together;
- We need a run-time value to construct a particular dependency.

## [Example](https://www.javaguides.net/2023/10/abstract-factory-design-pattern-in-kotlin.html#:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps <sup>[7](https://www.javaguides.net/2023/10/abstract-factory-design-pattern-in-kotlin.html#:~:text=Implementation%20Steps,specific%20UI%20components.)</sup>:
- Define abstract product interfaces for different types of UI components (e.g., Button, Window).
- Define concrete product classes for each product type and theme.
- Define an abstract factory interface that declares creation methods for each product type.
- Implement concrete factories for each theme that produce theme-specific UI components.

```
// Abstract product interfaces
interface Button {
    fun display(): String
}
interface Window {
    fun display(): String
}
// Concrete products for Light theme
class LightButton : Button {
    override fun display() = "Displaying Light Theme Button"
}
class LightWindow : Window {
    override fun display() = "Displaying Light Theme Window"
}
// Concrete products for Dark theme
class DarkButton : Button {
    override fun display() = "Displaying Dark Theme Button"
}
class DarkWindow : Window {
    override fun display() = "Displaying Dark Theme Window"
}
// Abstract Factory
interface GUIFactory {
    fun createButton(): Button
    fun createWindow(): Window
}
// Concrete factories
class LightThemeFactory : GUIFactory {
    override fun createButton() = LightButton()
    override fun createWindow() = LightWindow()
}
class DarkThemeFactory : GUIFactory {
    override fun createButton() = DarkButton()
    override fun createWindow() = DarkWindow()
}
fun main() {
    // Client code using the Abstract Factory
    val lightFactory: GUIFactory = LightThemeFactory()
    val lightButton: Button = lightFactory.createButton()
    println(lightButton.display())
    val lightWindow: Window = lightFactory.createWindow()
    println(lightWindow.display())
    val darkFactory: GUIFactory = DarkThemeFactory()
    val darkButton: Button = darkFactory.createButton()
    println(darkButton.display())
    val darkWindow: Window = darkFactory.createWindow()
    println(darkWindow.display())
}
```

output:
```
Displaying Light Theme Button
Displaying Light Theme Window
Displaying Dark Theme Button
Displaying Dark Theme Window
```

Explanation<sup>[8](https://www.javaguides.net/2023/10/abstract-factory-design-pattern-in-kotlin.html#:~:text=Explanation%3A,the%20chosen%20theme.)</sup>:
- `Button` and `Window` are abstract product interfaces representing UI components;
- `LightButton`, `LightWindow`, `DarkButton`, and `DarkWindow` are concrete implementations of these products, tailored for specific themes;
- `GUIFactory` is the Abstract Factory interface which declares methods for creating products;
- `LightThemeFactory` and `DarkThemeFactory` are concrete factories that instantiate theme-specific UI components;
- The client code (`main` function) uses an abstract factory to create UI components, ensuring they match the chosen theme.

## [Pros and Cons](https://swiftlynomad.medium.com/the-abstract-factory-pattern-in-swift-a-comprehensive-guide-5b018c2b1831#:~:text=your%20client%20code.-,Pros%20and%20Cons,-Pros%3A)
Pros:
- Ensures that created objects are compatible and belong to the same family;
- Promotes decoupling, making it easier to introduce new families of objects without modifying existing code;
- Enforces a consistent interface for creating objects, enhancing maintainability and readability.

Cons:
- Can lead to a large number of concrete classes if there are many families of objects;
- Can be overly complex for simple systems.

# Links
[Abstract Factory Pattern in Kotlin](https://www.baeldung.com/kotlin/abstract-factory-pattern)

[Abstract factory pattern](https://en.wikipedia.org/wiki/Abstract_factory_pattern)

[Abstract Factory Pattern in Java](https://www.baeldung.com/java-abstract-factory-pattern)

[Abstract Factory Design Pattern in Kotlin](https://www.javaguides.net/2023/10/abstract-factory-design-pattern-in-kotlin.html)

[The Abstract Factory Pattern in Swift: A Comprehensive Guide](https://swiftlynomad.medium.com/the-abstract-factory-pattern-in-swift-a-comprehensive-guide-5b018c2b1831)

# Further reading
[Abstract Factory Design Pattern](https://sourcemaking.com/design_patterns/abstract_factory)

[Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory)

[Abstract Factory Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/abstract-factory-software-pattern-kotlin-example)

[Kotlin Abstract Factory](https://swiderski.tech/kotlin-abstract-factory/)
