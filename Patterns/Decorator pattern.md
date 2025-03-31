# Decorator pattern
The Decorator Design Pattern is a structural design pattern that allows behavior to be added to individual objects dynamically, without affecting the behavior of other objects from the same class. It involves creating a set of decorator classes that are used to wrap concrete components.<sup>[1](https://www.geeksforgeeks.org/decorator-pattern/#:~:text=03%20Jan%2C%202025-,The%20Decorator%20Design%20Pattern%20is%20a%20structural%20design%20pattern%20that%20allows%20behavior%20to%20be%20added%20to%20individual%20objects%20dynamically%2C%20without%20affecting%20the%20behavior%20of%20other%20objects%20from%20the%20same%20class.%20It%20involves%20creating%20a%20set%20of%20decorator%20classes%20that%20are%20used%20to%20wrap%20concrete%20components.,-Important%20Topics%20for)</sup>

The decorator pattern provides a flexible alternative to subclassing for extending functionality. When using subclassing, different subclasses extend a class in different ways. However, an extension is bound to the class at compile-time and can't be changed at run-time. The decorator pattern allows responsibilities to be added (and removed from) an object dynamically at run-time. It is achieved by defining `Decorator` objects that:
- implement the interface of the extended (decorated) object (`Component`) transparently by forwarding all requests to it;
- perform additional functionality before or after forwarding a request.

This allows working with different Decorator objects to extend the functionality of an object dynamically at run-time.<sup>[2](https://en.wikipedia.org/wiki/Decorator_pattern#:~:text=The%20decorator%20pattern%20provides,time.%5B5%5D)</sup>

The Decorator Pattern works best when you need to add features to objects without changing their core structure. Use it for:<sup>[3](https://daily.dev/blog/decorator-pattern-explained-basics-to-advanced#:~:text=The%20Decorator%20Pattern%20works,a%20explosion%20of%20subclasses)</sup>
- Adding or removing behaviors at runtime;
- Enhancing legacy systems without touching their code;
- Avoiding a explosion of subclasses.

Don't use it if:<sup>[4](https://daily.dev/blog/decorator-pattern-explained-basics-to-advanced#:~:text=Don%27t%20use%20it,small%2C%20similar%20classes)</sup>
- Objects' core functionality changes often;
- You need to modify object internals;
- You already have many small, similar classes.

## [Example](https://www.javaguides.net/2023/10/decorator-design-pattern-in-kotlin.html#google_vignette:~:text=Implementation%20in%20Kotlin%20Programming)
Implementation steps:<sup>[5](https://www.javaguides.net/2023/10/decorator-design-pattern-in-kotlin.html#google_vignette:~:text=5.%20Implementation%20Steps,the%20abstract%20decorator.)</sup>
- Define an interface for the main object;
- Create concrete components implementing the main object interface;
- Create abstract decorator classes that also implement the main object interface;
- Implement concrete decorators by inheriting from the abstract decorator.

```
// Main object interface
interface Coffee {
    fun cost(): Double
    fun description(): String
}
// Concrete component
class BasicCoffee : Coffee {
    override fun cost() = 2.0
    override fun description() = "Basic Coffee"
}
// Abstract decorator
abstract class CoffeeDecorator(private val coffee: Coffee) : Coffee {
    override fun cost() = coffee.cost()
    override fun description() = coffee.description()
}
// Concrete decorators
class MilkDecorator(coffee: Coffee) : CoffeeDecorator(coffee) {
    override fun cost() = super.cost() + 0.5
    override fun description() = super.description() + ", Milk"
}
class SugarDecorator(coffee: Coffee) : CoffeeDecorator(coffee) {
    override fun cost() = super.cost() + 0.2
    override fun description() = super.description() + ", Sugar"
}
fun main() {
    val coffee: Coffee = SugarDecorator(MilkDecorator(BasicCoffee()))
    println("${coffee.description()} costs \$${coffee.cost()}")
}
```

Output:
```
Basic Coffee, Milk, Sugar costs $2.7
```

Explanation:<sup>[6](https://www.javaguides.net/2023/10/decorator-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,layers%20of%20decorators.)</sup>
- `Coffee` is the main object interface, defining methods to get the cost and description;
- `BasicCoffee` is a concrete component that implements the `Coffee` interface;
- `CoffeeDecorator` is an abstract decorator that implements the `Coffee` interface and wraps another `Coffee` object;
- `MilkDecorator` and `SugarDecorator` are concrete decorators that add their respective features to the coffee;
- In the `main` function, a coffee with milk and sugar is created using decorators. The final cost and description are calculated based on all layers of decorators.

## [Advantages of the Decorator Design Pattern](https://www.geeksforgeeks.org/decorator-pattern/#:~:text=2.7-,Advantages%20of%20the%20Decorator%20Design%20Pattern,-Here%20are%20some)
- **Open-Closed Principle**. The decorator pattern follows the open-closed principle, which states that classes should be open for extension but closed for modification. This means you can introduce new functionality to an existing class without changing its source code;
- **Flexibility**. It allows you to add or remove responsibilities (i.e., behaviors) from objects at runtime. This flexibility makes it easy to create complex object structures with varying combinations of behaviors;
- **Reusable Code**. Decorators are reusable components. You can create a library of decorator classes and apply them to different objects and classes as needed, reducing code duplication;
- **Composition over Inheritance**. Unlike traditional inheritance, which can lead to a deep and inflexible class hierarchy, the decorator pattern uses composition. You can compose objects with different decorators to achieve the desired functionality, avoiding the drawbacks of inheritance, such as tight coupling and rigid hierarchies;
- **Dynamic Behavior Modification**. Decorators can be applied or removed at runtime, providing dynamic behavior modification for objects. This is particularly useful when you need to adapt an object’s behavior based on changing requirements or user preferences;
- **Clear Code Structure**. The Decorator pattern promotes a clear and structured design, making it easier for developers to understand how different features and responsibilities are added to objects.

## [Disadvantages of the Decorator Design Pattern](https://www.geeksforgeeks.org/decorator-pattern/#:~:text=added%20to%20objects.-,Disadvantages%20of%20the%20Decorator%20Design%20Pattern,-Here%20are%20some)
- **Complexity**. As you add more decorators to an object, the code can become more complex and harder to understand. The nesting of decorators can make the codebase difficult to navigate and debug, especially when there are many decorators involved;
- **Increased Number of Classes**. When using the Decorator pattern, you often end up with a large number of small, specialized decorator classes. This can lead to a proliferation of classes in your codebase, which may increase maintenance overhead;
- **Order of Decoration**. The order in which decorators are applied can affect the final behavior of the object. If decorators are not applied in the correct order, it can lead to unexpected results. Managing the order of decorators can be challenging, especially in complex scenarios;
- **Potential for Overuse**. Because it’s easy to add decorators to objects, there is a risk of overusing the Decorator pattern, making the codebase unnecessarily complex. It’s important to use decorators judiciously and only when they genuinely add value to the design.

# Links
[Decorator Design Pattern](https://www.geeksforgeeks.org/decorator-pattern/)

[Decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern)

[Decorator Pattern Explained: Basics to Advanced](https://daily.dev/blog/decorator-pattern-explained-basics-to-advanced)

[Decorator Design Pattern in Kotlin](https://www.javaguides.net/2023/10/decorator-design-pattern-in-kotlin.html#google_vignette)

# Further reading
[Decorator Design Pattern](https://sourcemaking.com/design_patterns/decorator)

[Decorator](https://refactoring.guru/design-patterns/decorator)

[Decorator Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/decorator-software-pattern-kotlin-example)

[Decorator Pattern in Jetpack Compose Android Apps](https://www.blog.finotes.com/post/decorator-pattern-in-jetpack-compose-android-apps)

[Decorator Pattern in Kotlin](https://swiderski.tech/kotlin-decorator-pattern/)
