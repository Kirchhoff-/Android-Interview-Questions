# Strategy pattern
The Strategy Design Pattern is a behavioral design pattern that allows you to define a family of algorithms or behaviors, put each of them in a separate class, and make them interchangeable at runtime. This pattern is useful when you want to dynamically change the behavior of a class without modifying its code.<sup>[1](https://www.geeksforgeeks.org/system-design/strategy-pattern-set-1/#:~:text=The%20Strategy%20Design%20Pattern%20is,class%20without%20modifying%20its%20code.)</sup>

This pattern exhibits several key characteristics, such as:<sup>[2](https://www.geeksforgeeks.org/system-design/strategy-pattern-set-1/#:~:text=This%20pattern%20exhibits,a%20strategy%20object.)</sup>
- **Defines a family of algorithms**. The pattern allows you to encapsulate multiple algorithms or behaviors into separate classes, known as strategies;
- **Encapsulates behaviors**. Each strategy encapsulates a specific behavior or algorithm, providing a clean and modular way to manage different variations or implementations;
- **Enables dynamic behavior switching**. The pattern enables clients to switch between different strategies at runtime, allowing for flexible and dynamic behavior changes;
- **Promotes object collaboration**. The pattern encourages collaboration between a context object and strategy objects, where the context delegates the execution of a behavior to a strategy object.

The Strategy Design Pattern can be useful in various scenarios, such as:<sup>[3](https://www.freecodecamp.org/news/a-beginners-guide-to-the-strategy-design-pattern/#:~:text=The%20Strategy%20Design%20Pattern%20can%20be,object%20that%20needs%20to%20process%20payments.)</sup>
- **Sorting algorithms**. Different sorting algorithms can be encapsulated into separate strategies and passed to an object that needs sorting;
- **Validation rules**. Different validation rules can be encapsulated into separate strategies and passed to an object that needs validation;
- **Text formatting**. Different formatting strategies can be encapsulated into separate strategies and passed to an object that needs formatting;
- **Database access**. Different database access strategies can be encapsulated into separate strategies and passed to an object that needs to access data from different sources;
- **Payment strategy**. Different payment methods can be encapsulated into separate strategies and passed to an object that needs to process payments.

Real-World Use Cases:<sup>[4](https://www.javaguides.net/2023/10/strategy-design-pattern-in-kotlin.html#google_vignette:~:text=4.%20Real%2DWorld,multiple%20payment%20strategies.)</sup>
- Navigation  apps providing different routes based on travel mode: driving, biking, walking;
- Image compression applications where users can choose different compression algorithms;
- Payment gateways in e-commerce applications allow multiple payment strategies.

## [Example](https://www.javaguides.net/2023/10/strategy-design-pattern-in-kotlin.html#google_vignette:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[5](https://www.javaguides.net/2023/10/strategy-design-pattern-in-kotlin.html#google_vignette:~:text=5.%20Implementation%20Steps,switch%20between%20strategies.)</sup>
- Define a strategy interface common to all supported algorithms;
- Implement concrete strategies for the various algorithms;
- Define a context class to maintain a reference to a strategy object and switch between strategies.

```
// Step 1: Strategy Interface
interface RouteStrategy {
    fun buildRoute(start: String, destination: String): String
}
// Step 2: Concrete Strategies
class DrivingStrategy : RouteStrategy {
    override fun buildRoute(start: String, destination: String): String {
        return "Driving route from $start to $destination"
    }
}
class WalkingStrategy : RouteStrategy {
    override fun buildRoute(start: String, destination: String): String {
        return "Walking route from $start to $destination"
    }
}
// Step 3: Context Class
class Navigator(private var strategy: RouteStrategy) {
    fun setStrategy(newStrategy: RouteStrategy) {
        strategy = newStrategy
    }
    fun buildRoute(start: String, destination: String): String {
        return strategy.buildRoute(start, destination)
    }
}
// Client Code
fun main() {
    val navigator = Navigator(DrivingStrategy())
    println(navigator.buildRoute("Point A", "Point B"))
    navigator.setStrategy(WalkingStrategy())
    println(navigator.buildRoute("Point A", "Point B"))
}
```

Output:
```
Driving route from Point A to Point B
Walking route from Point A to Point B
```

Explanation:<sup>[6](https://www.javaguides.net/2023/10/strategy-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,the%20navigator%27s%20implementation.)</sup>
- `RouteStrategy` defines a contract that all strategies (algorithms) must follow;
- Concrete strategies (`DrivingStrategy`, `WalkingStrategy`) encapsulate the specific algorithms;
- `Navigator`, the context class, maintains a reference to a strategy object and can switch between strategies using `setStrategy`;
- The client can use the `Navigator` class and easily switch between different routing strategies without changing the navigator's implementation.

## Pros and Cons
Pros:<sup>[7](https://blog.evanemran.info/understanding-the-strategy-pattern-in-android-development#:~:text=the%20existing%20code.-,Benefits%20of%20Using%20the%20Strategy%20Pattern,in%20separate%20classes%20makes%20the%20codebase%20easier%20to%20maintain%20and%20extend.,-Drawbacks%20of%20the)</sup>
- Eliminates Conditional Logic: Reduces the need for complex conditional statements, making the code cleaner and more readable;
- Enhances Flexibility: Allows for easy addition or modification of algorithms without affecting the client code;
- Promotes Reusability: Each algorithm is a standalone class, promoting reuse in different parts of the application;
- Improves Maintainability: Encapsulating algorithms in separate classes makes the codebase easier to maintain and extend.

Cons:<sup>[8](https://blog.evanemran.info/understanding-the-strategy-pattern-in-android-development#:~:text=maintain%20and%20extend.-,Drawbacks%20of%20the%20Strategy%20Pattern,introduce%20overhead%20in%20scenarios%20where%20a%20simple%20conditional%20statement%20would%20suffice.,-Implementing%20the%20Strategy)</sup>
- Increased Number of Classes: Each algorithm requires a separate class, which can lead to an increase in the number of classes;
- Complexity: The pattern can introduce complexity, especially when there are numerous strategies;
- Overhead: The pattern may introduce overhead in scenarios where a simple conditional statement would suffice.

# Links
[Strategy Design Pattern](https://www.geeksforgeeks.org/system-design/strategy-pattern-set-1/)

[A Beginner's Guide to the Strategy Design Pattern](https://www.freecodecamp.org/news/a-beginners-guide-to-the-strategy-design-pattern/)

[Strategy Design Pattern in Kotlin](https://www.javaguides.net/2023/10/strategy-design-pattern-in-kotlin.html#google_vignette)

[Understanding the Strategy Pattern in Android Development](https://blog.evanemran.info/understanding-the-strategy-pattern-in-android-development)

# Further reading
[Strategy Design Pattern](https://sourcemaking.com/design_patterns/strategy)

[Strategy](https://refactoring.guru/design-patterns/strategy)

[Strategy Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/strategy-software-pattern-kotlin-example)

[Strategy Pattern in Kotlin](https://swiderski.tech/kotlin-strategy-pattern/)

[The Strategy Pattern](https://ersantana.com/coding/kotlin/design-patterns/strategy_pattern)
