# Bridge pattern
Bridge is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

This pattern involves an interface which acts as a bridge which makes the functionality of concrete classes independent from interface implementer classes. Both types of classes can be altered structurally without affecting.<sup>[1](https://dev.to/jps27cse/software-design-pattern-bridge-pattern-43f8#:~:text=Bridge%20is%20a,structurally%20without%20affecting)</sup>

The Bridge design pattern solves problems like:
- An abstraction and its implementation should be defined and extended independently from each other.
- A compile-time binding between an abstraction and its implementation should be avoided so that an implementation can be selected at run-time.<sup>[2](https://en.wikipedia.org/wiki/Bridge_pattern#:~:text=An%20abstraction%20and,at%20run%2Dtime.)</sup>

## [When to Use Bridge Method Design Pattern?](https://www.geeksforgeeks.org/bridge-method-design-pattern-in-java/#:~:text=maintainable%20and%20understandable.-,When%20to%20Use%20Bridge%20Method%20Design%20Pattern,-in%20Java)
- When you need to avoid a permanent binding between an abstraction and its implementation;
- When both the abstraction and implementation should be extensible through subclassing;
- When changes in the implementation of an abstraction should not impact the clients;
- When you want to share an implementation among multiple objects and hide implementation details from the clients.

## [Key Components of the Bridge Pattern](https://medium.com/@thecodebean/bridge-design-pattern-implementation-in-java-f71c853979fe#:~:text=Key%20Components%20of%20the%20Bridge%20Pattern)
- **Abstraction**. This is the high-level interface that defines the abstract methods or operations that the clients will use;
- **Implementor**. This is the interface or abstract class that defines the methods that the concrete implementors must implement;
- **Concrete Abstraction**. These are concrete classes that extend the Abstraction and use an Implementor to perform specific operations;
- **Concrete Implementor**. These are concrete classes that implement the Implementor interface and provide actual implementations of the methods defined in the Implementor.

## [Example](https://www.javaguides.net/2023/10/bridge-design-pattern-in-kotlin.html#:~:text=Implementation%20in%20Kotlin%20Programming)
```
// Implementor
interface Color {
    fun applyColor(): String
}
class RedColor : Color {
    override fun applyColor() = "Red"
}
class BlueColor : Color {
    override fun applyColor() = "Blue"
}
// Abstraction
abstract class Shape(protected val color: Color) {
    abstract fun draw(): String
}
class Circle(color: Color) : Shape(color) {
    override fun draw() = "Circle drawn in ${color.applyColor()} color"
}
class Square(color: Color) : Shape(color) {
    override fun draw() = "Square drawn in ${color.applyColor()} color"
}
fun main() {
    val redCircle = Circle(RedColor())
    val blueSquare = Square(BlueColor())
    println(redCircle.draw())
    println(blueSquare.draw())
}
```

Output:
```
Circle drawn in Red color
Square drawn in Blue color
```

- `Color` is the implementor interface with different concrete classes like `RedColor` and `BlueColor`;
- `Shape` is the abstraction that takes a `Color` as a parameter, which provides the concrete implementation of the color;
- Concrete classes like `Circle` and `Square` extend `Shape` and use the provided color to draw themselves;
- In the `main` function, we demonstrate the decoupled nature by drawing a red circle and a blue square without having specific classes for each combination.

## [Advantages of Bridge Method Design Pattern](https://www.geeksforgeeks.org/bridge-method-design-pattern-in-java/#:~:text=the%20operationImpl()%20method.-,Advantages%20of%20Bridge%20Method%20Design%20Pattern,-in%20Java)
- **Decoupling Abstraction and Implementation**. The pattern decouples the abstraction from its implementation, allowing them to be developed and modified independently;
- **Improved Extensibility**. It is easier to extend both abstraction and implementation hierarchies independently;
- **Enhanced Flexibility**. The pattern allows changing the implementation without modifying the abstraction and vice versa;
- **Simplified Code Maintenance**. By separating the concerns, the code becomes more maintainable and understandable.

# Links
[Software Design Pattern: Bridge Pattern](https://dev.to/jps27cse/software-design-pattern-bridge-pattern-43f8)

[Bridge Design Pattern: Implementation in Java](https://medium.com/@thecodebean/bridge-design-pattern-implementation-in-java-f71c853979fe)

[Bridge Design Pattern in Kotlin](https://www.javaguides.net/2023/10/bridge-design-pattern-in-kotlin.html)

[Bridge Method Design Pattern in Java](https://www.geeksforgeeks.org/bridge-method-design-pattern-in-java/)

# Further reading
[Bridge Pattern](https://springframework.guru/gang-of-four-design-patterns/bridge-pattern/)

[Bridge](https://refactoring.guru/design-patterns/bridge)

[Bridge Design Pattern in Android](https://medium.com/@manishkumar_75473/bridge-design-pattern-in-android-66cfc67e36fd)

[Mastering the Bridge Design Pattern in Kotlin: A Comprehensive Guide](https://softaai.com/mastering-the-bridge-design-pattern-in-kotlin/)

[Bridge Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/bridge-software-pattern-kotlin-example)
