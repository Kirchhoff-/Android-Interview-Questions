# Visitor pattern
The Visitor design pattern is a behavioral pattern that allows you to add new operations to a group of related classes without modifying their structures. It is particularly useful when you have a stable set of classes but need to perform various operations on them, making it easy to extend functionality without altering the existing codebase.<sup>[1](https://www.geeksforgeeks.org/system-design/visitor-design-pattern/#:~:text=The%20Visitor%20design%20pattern%20is,without%20altering%20the%20existing%20codebase.)</sup>

What problems can the Visitor design pattern solve?<sup>[2](https://en.wikipedia.org/wiki/Visitor_pattern#:~:text=Problems%2C%20the%20Visitor,changing%20the%20classes.)</sup>
- It should be possible to define a new operation for (some) classes of an object structure without changing the classes.

What solution does the Visitor design pattern describe?<sup>[3](https://en.wikipedia.org/wiki/Visitor_pattern#:~:text=Solution%2C%20the%20Visitor,visits%20the%20element%22).)</sup>
- Define a separate (visitor) object that implements an operation to be performed on elements of an object structure;
- Clients traverse the object structure and call a dispatching operation accept (visitor) on an element — that "dispatches" (delegates) the request to the "accepted visitor object". The visitor object then performs the operation on the element ("visits the element").

This makes it possible to create new operations independently from the classes of an object structure by adding new visitor objects.<sup>[4](https://en.wikipedia.org/wiki/Visitor_pattern#:~:text=This%20makes%20it%20possible%20to%20create%20new%20operations%20independently%20from%20the%20classes%20of%20an%20object%20structure%20by%20adding%20new%20visitor%20objects.)</sup>

Motivation for Using the Visitor Pattern:<sup>[5](https://medium.com/@alxkm/visitor-pattern-in-java-0be5fa5af5d7#:~:text=Motivation%20for%20Using,or%20vice%20versa.)</sup>
- **Separation of Concerns**. The Visitor Pattern promotes the separation of concerns by separating the algorithm from the object structure it operates on. This makes the system more modular and easier to understand;
- **Extensibility**. It allows you to define new operations without changing the classes of the elements on which it operates. This makes it easy to add new functionality as the system evolves without modifying existing code;
- **Improved Maintainability**. By using the Visitor Pattern, you can centralize related behavior in a visitor rather than scattering it across the object structure. This reduces code duplication and improves maintainability;
- **Enhanced Flexibility**. It provides a way to define a new operation without changing the classes of the elements on which it operates, which is particularly useful when dealing with a complex object structure;
- **Decoupling**. It decouples the operations from the object structure, making it easier to change the object structure without impacting the operations, or vice versa.

When to use?<sup>[6](https://www.javaguides.net/2023/10/visitor-design-pattern-in-kotlin.html#google_vignette:~:text=7.%20When%20to,them%20without%20modification.)</sup>
- You need to add further operations to objects without modifying their classes;
- A structure contains many unrelated operations, and grouping them in the same class isn't feasible;
- Classes defining the objects that you're going to work with aren't likely to change, but you want to define new operations on them without modification.

## [Example](https://www.javaguides.net/2023/10/visitor-design-pattern-in-kotlin.html#google_vignette:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[7](https://www.javaguides.net/2023/10/visitor-design-pattern-in-kotlin.html#google_vignette:~:text=5.%20Implementation%20Steps,the%20accept%20method.)</sup>
- Define a visitor interface declaring visit methods for each element class;
- Concrete visitor classes implement this interface;
- Element classes offer an 'accept' method that takes a visitor as an argument;
- The client passes the visitor to elements using the accept method.

```
// Step 1: Element Interface and Concrete Elements
interface Shape {
    fun accept(visitor: ShapeVisitor)
}
class Circle(val radius: Double) : Shape {
    override fun accept(visitor: ShapeVisitor) {
        visitor.visit(this)
    }
}
class Square(val side: Double) : Shape {
    override fun accept(visitor: ShapeVisitor) {
        visitor.visit(this)
    }
}
// Step 2: Visitor Interface and Concrete Visitors
interface ShapeVisitor {
    fun visit(circle: Circle)
    fun visit(square: Square)
}
class AreaVisitor : ShapeVisitor {
    override fun visit(circle: Circle) {
        println("Area of Circle: ${Math.PI * circle.radius * circle.radius}")
    }
    override fun visit(square: Square) {
        println("Area of Square: ${square.side * square.side}")
    }
}
// Client Code
fun main() {
    val shapes: List<Shape> = listOf(Circle(5.0), Square(4.0))
    val visitor = AreaVisitor()
    for (shape in shapes) {
        shape.accept(visitor)
    }
}
```

Output:
```
Area of Circle: 78.53981633974483
Area of Square: 16.0
```

Explanation:<sup>[8](https://www.javaguides.net/2023/10/visitor-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,the%20desired%20operation.)</sup>
- `Shape` is the element interface with an `accept` method that takes a visitor;
- `Circle` and `Square` are concrete elements. They implement the `accept` method, passing themselves to the visitor's `visit` method;
- `ShapeVisitor` is the visitor interface with a `visit` method for each element type;
- `AreaVisitor` is a concrete visitor that calculates the area for each shape.
- In the client code, shapes accept the visitor, which then performs the desired operation.

## Pros and Cons
Pros:<sup>[9](https://www.geeksforgeeks.org/system-design/visitor-design-pattern/#:~:text=Separation%20of%20Concerns,operations%20are%20applied.)</sup>
- **Separation of Concerns**. This pattern keeps operations separate from the objects themselves, making it easier to manage and understand the code;
- **Easy to Add New Features**. You can introduce new operations simply by creating new visitor classes without changing the existing objects. This makes the system flexible;
- **Centralized Logic**. All the operations are in one place (the visitor), which helps you see how different tasks interact with your objects;
- **Easier Maintenance**. If you need to update or fix something, you can do it in the visitor class without touching the object classes, making maintenance simpler;
- **Type Safety**. Each visitor method is specific to an object type, which helps catch errors early and ensures the right operations are applied.

Cons:<sup>[10](https://www.geeksforgeeks.org/system-design/visitor-design-pattern/#:~:text=Added%20Complexity%3A%20It,classes%20each%20time.)</sup>
- **Added Complexity**. It can make your code more complicated, especially if you have many types of objects or operations to manage;
- **Challenging to Add New Objects**. While adding new operations is easy, introducing new types of objects requires changes to all visitor classes, which can be a hassle;
- **Tight Coupling**. Visitors need to know about all the specific object types, which can create a dependency and make your design less flexible;
- **More Classes to Manage**. This pattern can lead to a lot of extra classes and interfaces, which can clutter your codebase and make it harder to navigate;
- **Not Ideal for Frequent Changes**. If your object types change often, the Visitor pattern can become a burden, as you'd need to update multiple visitor classes each time.

# Links
[Visitor design pattern](https://www.geeksforgeeks.org/system-design/visitor-design-pattern/)

[Visitor pattern](https://en.wikipedia.org/wiki/Visitor_pattern)

[Visitor Pattern in Java](https://medium.com/@alxkm/visitor-pattern-in-java-0be5fa5af5d7)

[Visitor Design Pattern in Kotlin](https://www.javaguides.net/2023/10/visitor-design-pattern-in-kotlin.html#google_vignette)

# Further reading
[Visitor Design Pattern](https://sourcemaking.com/design_patterns/visitor)

[Visitor](https://refactoring.guru/design-patterns/visitor)

[Visitor Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/visitor-software-pattern-kotlin-example)

[Visitor Design Pattern – Fostering Dynamic Object Operations](https://neatcode.org/visitor-pattern/)
