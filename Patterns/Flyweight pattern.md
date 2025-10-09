# Flyweight pattern
The Flyweight design pattern is a structural pattern that optimizes memory usage by sharing a common state among multiple objects. It aims to reduce the number of objects created and to decrease memory footprint, which is particularly useful when dealing with a large number of similar objects.<sup>[1](https://www.geeksforgeeks.org/system-design/flyweight-design-pattern/#:~:text=The%20Flyweight%20design%20pattern%20is%20a%20structural%20pattern%20that%20optimizes%20memory%20usage%20by%20sharing%20a%20common%20state%20among%20multiple%20objects.%20It%20aims%20to%20reduce%20the%20number%20of%20objects%20created%20and%20to%20decrease%20memory%20footprint%2C%20which%20is%20particularly%20useful%20when%20dealing%20with%20a%20large%20number%20of%20similar%20objects.)</sup>

The Flyweight pattern suggests creating a common interface for flyweight objects and then segregating the intrinsic (shared) and extrinsic (unique) states. Shared data is stored in flyweight objects, while unique data is stored or computed externally and passed to the flyweights when needed.<sup>[2](https://www.javaguides.net/2023/10/flyweight-design-pattern-in-kotlin.html#:~:text=The%20Flyweight%20pattern%20suggests%20creating%20a%20common%20interface%20for%20flyweight%20objects%20and%20then%20segregating%20the%20intrinsic%20(shared)%20and%20extrinsic%20(unique)%20states.%20Shared%20data%20is%20stored%20in%20flyweight%20objects%2C%20while%20unique%20data%20is%20stored%20or%20computed%20externally%20and%20passed%20to%20the%20flyweights%20when%20needed.)</sup>

When to use Flyweight Design Pattern?<sup>[3](https://www.geeksforgeeks.org/system-design/flyweight-design-pattern/#:~:text=at%20po...-,When%20to%20use%20Flyweight%20Design%20Pattern,collection%2C%20object%20generation%2C%20and%20destruction%20by%20minimizing%20the%20number%20of%20objects.,-When%20not%20to)</sup>
- **When you need to create a large number of similar objects**. For instance, suppose you have to make hundreds of graphical objects (such as buttons and icons) with similar attributes (such as color and picture) but different positions and other variable features;
- **When memory consumption is a concern**. By sharing common state, the Flyweight pattern can assist reduce the memory footprint of memory-intensive applications that would otherwise require a large amount of RAM to create several instances of comparable objects;
- **When performance optimization is needed**. The pattern can enhance efficiency by lowering the overhead related to trash collection, object generation, and destruction by minimizing the number of objects.

When not to use Flyweight Design Pattern?<sup>[4](https://www.geeksforgeeks.org/system-design/flyweight-design-pattern/#:~:text=number%20of%20objects.-,When%20not%20to%20use%20Flyweight%20Design%20Pattern,implementing%20the%20Flyweight%20pattern%20may%20add%20unnecessary%20complexity%20without%20significant%20benefits.,-Conclusion)</sup>
- **When objects have unique intrinsic states**. If each object instance requires a unique set of intrinsic state that cannot be shared with other objects, applying the Flyweight pattern may not provide significant benefits;
- **When the overhead of implementing the pattern outweighs the benefits**. If the number of objects is relatively small or the shared state is minimal, the complexity introduced by the Flyweight pattern may not justify its implementation;
- **When mutable objects are involved**. The Flyweight pattern is best suited for immutable objects or objects whose intrinsic state does not change once initialized. If objects frequently change their intrinsic state, managing shared state can become complex and error-prone;
- **When the application does not have performance or memory constraints**. If memory usage and performance are not critical concerns for your application, implementing the Flyweight pattern may add unnecessary complexity without significant benefits.

## [Example](https://www.javaguides.net/2023/10/flyweight-design-pattern-in-kotlin.html#:~:text=Implementation%20in%20Kotlin%20Programming)
Implementation steps:<sup>[5](https://www.javaguides.net/2023/10/flyweight-design-pattern-in-kotlin.html#:~:text=5.%20Implementation%20Steps,and%20extrinsic%20states.)</sup>
- Identify the common states (intrinsic) and the unique states (extrinsic);
- Create a flyweight interface and concrete flyweights to encapsulate the shared states;
- The client code should differentiate between intrinsic and extrinsic states.

```
// Step 1: Flyweight Interface
interface Shape {
    fun draw(x: Int, y: Int, color: String)
}
// Step 2: Concrete Flyweight
class Circle(private val radius: Int) : Shape {
    override fun draw(x: Int, y: Int, color: String) {
        println("Drawing Circle at ($x,$y) with radius $radius and color $color")
    }
}
// Step 3: Flyweight Factory
object ShapeFactory {
    private val circleMap = mutableMapOf<Int, Circle>()
    fun getCircle(radius: Int): Circle {
        if (!circleMap.containsKey(radius)) {
            circleMap[radius] = Circle(radius)
        }
        return circleMap[radius]!!
    }
}
// Client Code
fun main() {
    val circle1 = ShapeFactory.getCircle(5)
    circle1.draw(10, 10, "Red")
    val circle2 = ShapeFactory.getCircle(5)
    circle2.draw(20, 20, "Blue")
    val circle3 = ShapeFactory.getCircle(10)
    circle3.draw(30, 30, "Green")
}
```

Output:
```
Drawing Circle at (10,10) with radius 5 and color Red
Drawing Circle at (20,20) with radius 5 and color Blue
Drawing Circle at (30,30) with radius 10 and color Green
```

Explanation:<sup>[6](https://www.javaguides.net/2023/10/flyweight-design-pattern-in-kotlin.html#:~:text=Explanation%3A,the%20same%20radius.)</sup>
- `Shape` is a flyweight interface with a `draw` method;
- `Circle` is a concrete flyweight storing the intrinsic state (radius) and taking the extrinsic state (position and color) as parameters in the `draw` method;
- `ShapeFactory` acts as a factory to create and manage flyweight objects. If a circle with a specific radius already exists, it is returned; otherwise, a new one is created;
- In the client code, circles are drawn at different positions and colors but share the same radius.

## Pros and Cons

Pros:<sup>[7](https://javaplanet.io/java-core/design-patterns/flyweight-pattern/#:~:text=Pros,the%20Flyweight%20Factory.)</sup>
- **Memory Efficiency**. Reduces memory usage by sharing common state across multiple objects;
- **Scalability**. Enables handling a large number of objects without significant resource overhead;
- **Maintainability**. Centralizes intrinsic state management in the Flyweight Factory.

Cons:<sup>[8](https://javaplanet.io/java-core/design-patterns/flyweight-pattern/#:~:text=Cons,avoid%20concurrency%20issues)</sup>
- **Complexity**. Introduces additional classes and logic for managing intrinsic and extrinsic state;
- **Trade-Offs**. May increase runtime overhead due to passing extrinsic state repeatedly;
- **Thread Safety**. Shared Flyweight objects must be immutable or thread-safe to avoid concurrency issues.

# Links
[Flyweight Design Pattern](https://www.geeksforgeeks.org/system-design/flyweight-design-pattern/)

[Flyweight Design Pattern in Kotlin](https://www.javaguides.net/2023/10/flyweight-design-pattern-in-kotlin.html)

[Flyweight Pattern](https://javaplanet.io/java-core/design-patterns/flyweight-pattern/)

# Further reading
[Flyweight Design Pattern](https://sourcemaking.com/design_patterns/flyweight)

[Flyweight](https://refactoring.guru/design-patterns/flyweight)

[Flyweight Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/flyweight-software-pattern-kotlin-example)
