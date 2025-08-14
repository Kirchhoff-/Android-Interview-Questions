# Factory method pattern
The Factory Method pattern is a creational design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. It defines a method for creating objects, which subclasses can then override to change the type of objects that will be created.<sup>[1](https://proandroiddev.com/zero-to-hero-in-android-kotlin-creational-design-patterns-a972375c352a#:~:text=The%20Factory%20Method%20pattern%20is%20a%20creational%20design%20pattern,change%20the%20type%20of%20objects%20that%20will%20be%20created.)</sup>

The factory method design pattern solves problems such as:<sup>[2](https://en.wikipedia.org/wiki/Factory_method_pattern#:~:text=The%20factory%20method%20design,directly%20calling%20a%20constructor.)</sup>
- How can an object's subclasses redefine its subsequent and distinct implementation? The pattern involves creation of a factory method within the superclass that defers the object's creation to a subclass's factory method;
- How can an object's instantiation be deferred to a subclass? Create an object by calling a factory method instead of directly calling a constructor.

This enables the creation of subclasses that can change the way in which an object is created (for example, by redefining which class to instantiate).<sup>[3](https://en.wikipedia.org/wiki/Factory_method_pattern#:~:text=This%20enables%20the%20creation%20of%20subclasses%20that%20can%20change%20the%20way%20in%20which%20an%20object%20is%20created%20(for%20example%2C%20by%20redefining%20which%20class%20to%20instantiate).)</sup>

When to use Factory Method Design Pattern?<sup>[4](https://www.geeksforgeeks.org/java/factory-method-design-pattern-in-java/#:~:text=at%20the%20implementation.-,When%20to%20use%20Factory%20Method%20Design%20Pattern%3F,helper%20subclass%20is%20the%20delegate%20within%20a%20specific%20scope%20or%20location.,-Key%20Components%20of)</sup>
- A class cannot predict the type of objects it needs to create;
- A class wants its subclasses to specify the objects it creates;
- Classes delegate responsibility to one of multiple helper subclasses, and you aim to keep the information about which helper subclass is the delegate within a specific scope or location.

## [Example](https://www.javaguides.net/2023/10/factory-method-design-pattern-in-kotlin.html#:~:text=6.%20Generic%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[5](https://www.javaguides.net/2023/10/factory-method-design-pattern-in-kotlin.html#:~:text=5.%20Implementation%20Steps,the%20new%20keyword.)</sup>
- Define an interface or abstract class with a factory method;
- Concrete classes will implement or override this factory method to produce objects;
- Clients will call the factory method instead of directly using the `new` keyword.

```
// Abstract product
interface Product {
    fun showProductType(): String
}
// Concrete products
class ConcreteProductA : Product {
    override fun showProductType() = "Product Type A"
}
class ConcreteProductB : Product {
    override fun showProductType() = "Product Type B"
}
// Creator class with the factory method
abstract class Creator {
    abstract fun factoryMethod(): Product
}
// Concrete creators that override the factory method
class ConcreteCreatorA : Creator() {
    override fun factoryMethod() = ConcreteProductA()
}
class ConcreteCreatorB : Creator() {
    override fun factoryMethod() = ConcreteProductB()
}
fun main() {
    // Client code
    val creatorA: Creator = ConcreteCreatorA()
    val productA: Product = creatorA.factoryMethod()
    println(productA.showProductType())
    val creatorB: Creator = ConcreteCreatorB()
    val productB: Product = creatorB.factoryMethod()
    println(productB.showProductType())
}
```

Output:
```
Product Type A
Product Type B
```

Explanation:<sup>[6](https://www.javaguides.net/2023/10/factory-method-design-pattern-in-kotlin.html#:~:text=Explanation%3A,get%20the%20product.)</sup>
- `Product` is an interface representing the abstract product;
- `ConcreteProductA` and `ConcreteProductB` are concrete implementations of the `Product`;
- `Creator` is an abstract class with an abstract `factoryMethod` which returns an object of type `Product`;
- `ConcreteCreatorA` and `ConcreteCreatorB` are subclasses of `Creator` that override the `factoryMethod` to produce `ConcreteProductA` and `ConcreteProductB` respectively;
- In the client code (`main` function), instead of directly instantiating the product, we use the concrete creators' `factoryMethod` to get the product.

## Pros and Cons
Pros:<sup>[7](https://www.geeksforgeeks.org/system-design/factory-method-for-designing-pattern/#:~:text=Advantages%20of%20the,clients%2C%20reducing%20dependency.)</sup>
- Separates creation logic from client code, improving flexibility;
- New product types can be added easily;
- Simplifies unit testing by allowing mock class creation;
- Centralizes object creation logic across the application;
- Hides specific product classes from clients, reducing dependency.

Cons:<sup>[8](https://www.geeksforgeeks.org/system-design/factory-method-for-designing-pattern/#:~:text=Disadvantages%20of%20the,applied%20too%20broadly.)</sup>
- Adds more classes and interfaces, which can complicate maintenance;
- Slight performance impacts due to polymorphism;
- Clients need knowledge of specific subclasses;
- May lead to unnecessary complexity if applied too broadly.

# Links
[Zero To Hero in Android Kotlin Creational Design Patterns](https://proandroiddev.com/zero-to-hero-in-android-kotlin-creational-design-patterns-a972375c352a)

[Factory method pattern](https://en.wikipedia.org/wiki/Factory_method_pattern)

[Factory Method Design Pattern in Java](https://www.geeksforgeeks.org/java/factory-method-design-pattern-in-java/)

[Factory Method Design Pattern in Kotlin](https://www.javaguides.net/2023/10/factory-method-design-pattern-in-kotlin.html)

[Factory method Design Pattern](https://www.geeksforgeeks.org/system-design/factory-method-for-designing-pattern/)

# Further reading
[Factory Method Design Pattern](https://sourcemaking.com/design_patterns/factory_method)

[Factory Method](https://refactoring.guru/design-patterns/factory-method)
