# Adapter pattern
The adapter pattern is a structural design pattern that permits two somewhat incompatible interfaces to work together. It acts as a bridge between two classes with different interfaces, allowing them to work together seamlessly.<sup>[1](https://www.baeldung.com/kotlin/adapter-pattern#:~:text=The%20adapter%20pattern%20is%20a%20structural%20design%20pattern%20that%20permits%20two%20somewhat%20incompatible%20interfaces%20to%20work%20together.%20It%20acts%20as%20a%20bridge%20between%20two%20classes%20with%20different%20interfaces%2C%20allowing%20them%20to%20work%20together%20seamlessly)</sup>

The adapter design pattern solves problems like<sup>[2](https://en.wikipedia.org/wiki/Adapter_pattern#:~:text=The%20adapter%20design,for%20a%20class%3F)</sup>:
- How can a class be reused that does not have an interface that a client requires?
- How can classes that have incompatible interfaces work together?
- How can an alternative interface be provided for a class?

Often an (already existing) class can not be reused only because its interface does not conform to the interface clients require.

The adapter design pattern describes how to solve such problems:
- Define a separate `adapter` class that converts the (incompatible) interface of a class (`adaptee`) into another interface (`target`) clients require;
- Work through an `adapter` to work with (reuse) classes that do not have the required interface.

The key idea in this pattern is to work through a separate `adapter` that adapts the interface of an (already existing) class without changing it.

Clients don't know whether they work with a `target` class directly or through an `adapter` with a class that does not have the `target` interface.<sup>[3](https://en.wikipedia.org/wiki/Adapter_pattern#:~:text=Often%20an%20\(already,the%20target%20interface.)</sup>

Key Characteristics<sup>[4](https://blog.evanemran.info/understanding-the-adapter-pattern-in-android-development#:~:text=changing%20their%20code.-,Key%20Characteristics%3A,it%20easy%20to%20introduce%20new%20types%20of%20data%20sources%20or%20components.,-Benefits%20of%20the)</sup>:
- **Interface Conversion**. Converts the interface of a class into another interface clients expect;
- **Reusability**. Promotes code reusability by allowing different classes to work together without modifying their code;
- **Flexibility**. Makes it easy to introduce new types of data sources or components.

## [When to use Adapter Design Pattern?](https://www.geeksforgeeks.org/adapter-pattern/#:~:text=even%20more%20difficult.-,When%20to%20use%20Adapter%20Design%20Pattern%3F,-Use%20adapter%20design)
Use adapter design pattern when:
- We need to connect systems or components that weren’t built to work together. The adapter allows these incompatible interfaces to communicate, making integration smoother;
- Many times, we have existing code or libraries that we want to use, but they don’t match our current system. The adapter helps us incorporate this old code without having to rewrite it;
- As projects grow, new components are frequently added. An adapter allows you to integrate these new pieces without affecting the existing code, keeping the system flexible and adaptable;
- By isolating the changes needed for compatibility in one place, the adapter makes it easier to maintain the code. This reduces the risk of bugs that might arise from changing multiple parts of the system.

## [Real life examples of Adapter Design Pattern](https://medium.com/@akshatsharma0610/adapter-design-pattern-in-java-fa20d6df25b8#:~:text=Real%20life%20examples%20of%20Adapter%20Design%20Pattern)
The Adapter pattern can be applied in various scenarios. Here are a few real-world examples:
- **Database Adapters**. When working with different database systems, each may have its own specific API. An adapter can be used to convert the operations and queries from one database system to another, allowing the client code to work with a common interface;
- **Legacy System Integration**. When integrating new software components with existing legacy systems, the Adapter pattern can be used to translate the legacy system’s interface into a more modern and compatible one;
- **Plug Adapters**. In electrical systems, different countries may have different types of electrical outlets. Plug adapters allow devices with one type of plug to be used with different outlet types by adapting the plug to fit the specific outlet.

## [Adapter Pattern in Android](https://blog.evanemran.info/understanding-the-adapter-pattern-in-android-development#heading-adapter-pattern-in-android)
In Android development, the Adapter Pattern is widely used for managing and displaying data in UI components such as `ListView`, `RecyclerView`, and `ViewPager`. Adapters act as a bridge between the UI components and the data source, converting data items into viewable elements. 

Common Use Cases:
- **RecyclerView Adapters**. Convert data into `ViewHolder` items for efficient display in a `RecyclerView`;
- **ListView Adapters**. Adapt data for `ListView` and `GridView` components;
- **ViewPager Adapters**. Adapt pages for `ViewPager` to display a series of fragments or views.

## [Example](https://www.javaguides.net/2023/10/adapter-design-pattern-in-kotlin.html#:~:text=Implementation%20in%20Kotlin%20Programming)
Implementation steps<sup>[5](https://www.javaguides.net/2023/10/adapter-design-pattern-in-kotlin.html#:~:text=Implementation%20Steps,delegating%20to%20the%20adaptee.)</sup>:
- Identify the existing interface that needs adapting (`Target`);
- Recognize the interface that the client expects (`Adaptee`);
- Create an adapter class that implements the target interface and has a reference to the adaptee;
- Implement the methods of the target interface in the adapter by delegating to the adaptee.

```
// Target interface
interface ThreePinSocket {
    fun acceptThreePinPlug()
}
// Adaptee
class TwoPinPlug {
    fun insertTwoPinPlug() {
        println("Two-pin plug inserted!")
    }
}
// Adapter
class PlugAdapter(private val plug: TwoPinPlug) : ThreePinSocket {
    override fun acceptThreePinPlug() {
        plug.insertTwoPinPlug()
        println("Adapter made it compatible with three-pin socket!")
    }
}
fun main() {
    val twoPinPlug = TwoPinPlug()
    val adapter = PlugAdapter(twoPinPlug)
    adapter.acceptThreePinPlug()
}
```

Output:
```
Two-pin plug inserted!
Adapter made it compatible with three-pin socket!
```

Explanation<sup>[6](https://www.javaguides.net/2023/10/adapter-design-pattern-in-kotlin.html#:~:text=Explanation%3A,to%20the%20socket.)</sup>:
- `ThreePinSocket` is the target interface that our client (in this case, the socket in the hotel) expects;
- `TwoPinPlug` is the adaptee – the existing functionality we want to adapt;
- `PlugAdapter` is the adapter class. It takes a `TwoPinPlug` as a reference and implements the `ThreePinSocket` interface. It makes the two-pin plug compatible with the three-pin socket;
- In the `main` function, we create a `TwoPinPlug` object and an adapter. We then use the adapter to connect the plug to the socket.

## [Pros of Adapter Design Pattern](https://www.geeksforgeeks.org/adapter-pattern/#:~:text=printing%20a%20document.-,Pros%20of%20Adapter%20Design%20Pattern,-Below%20are%20the)
- By creating an adapter, you can reuse existing code without needing to modify it. This promotes code reuse and helps maintain a cleaner architecture;
- By separating the issues of interface adaptation, the adapter pattern frees classes to concentrate on their main duties without having to deal with adaptation code that clogs their logic;
- Because you can simply switch out multiple adapters to support different interfaces without altering the underlying system;
- By separating your system from particular implementations, adapters make it simpler to swap out or modify parts without compromising the functionality of other parts.

## [Cons of Adapter Design Pattern](https://www.geeksforgeeks.org/adapter-pattern/#:~:text=of%20other%20parts.-,Cons%20of%20Adapter%20Design%20Pattern,-Below%20are%20the)
- Introducing adapters can add a layer of complexity to your system. Having multiple adapters can make the code harder to navigate and understand;
- The additional layer of indirection may introduce slight performance overhead, especially if the adapter needs to perform complex transformations;
- If not managed properly, the use of adapters can lead to maintenance challenges. Keeping track of multiple adapters for various interfaces can become cumbersome;
- There’s a risk of overusing adapters for trivial changes, which can lead to unnecessary complexity. It’s critical to assess whether an adapter is actually required in a particular circumstance;
- Only two interfaces can be translated by adapters; if you need to adjust to more than one interface, you might need a lot of different adapters, which could make the design even more difficult.

# Links
[The Adapter Pattern in Kotlin](https://www.baeldung.com/kotlin/adapter-pattern)

[Adapter pattern](https://en.wikipedia.org/wiki/Adapter_pattern)

[Understanding the Adapter Pattern in Android Development](https://blog.evanemran.info/understanding-the-adapter-pattern-in-android-development)

[Adapter Design Pattern](https://www.geeksforgeeks.org/adapter-pattern/)

[Adapter Design Pattern in Java](https://medium.com/@akshatsharma0610/adapter-design-pattern-in-java-fa20d6df25b8)

[Adapter Design Pattern in Kotlin](https://www.javaguides.net/2023/10/adapter-design-pattern-in-kotlin.html)

# Further reading
[Adapter](https://refactoring.guru/design-patterns/adapter)

[Adapter Design Pattern](https://sourcemaking.com/design_patterns/adapter)

[Adapter Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/adapter-software-pattern-kotlin-example)
