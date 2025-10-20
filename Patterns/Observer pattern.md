# Observer pattern
Observer Design Pattern is a behavioral pattern that establishes a one-to-many dependency between objects. When the subject changes its state, all its observers are automatically notified and updated. It focuses on enabling efficient communication and synchronization between objects in response to state changes.<sup>[1](https://www.geeksforgeeks.org/system-design/observer-pattern-set-1-introduction/#:~:text=Observer%20Design%20Pattern%20is,response%20to%20state%20changes.)</sup> 

The observer pattern addresses the following requirements:<sup>[2](https://en.wikipedia.org/wiki/Observer_pattern#:~:text=The%20observer%20pattern%20addresses,notify%20multiple%20other%20objects.)</sup> 
- A one-to-many dependency between objects should be defined without making the objects tightly coupled;
- When one object changes state, an open-ended number of dependent objects should be updated automatically;
- An object can notify multiple other objects.

The naive approach would be for one object (subject) to directly call specific methods on each dependent object. This creates tight coupling because the subject must know the concrete types and specific interfaces of all dependent objects, making the code inflexible and hard to extend.<sup>[3](https://en.wikipedia.org/wiki/Observer_pattern#:~:text=The%20naive%20approach%20would%20be%20for%20one%20object%20(subject)%20to%20directly%20call%20specific%20methods%20on%20each%20dependent%20object.%20This%20creates%20tight%20coupling%20because%20the%20subject%20must%20know%20the%20concrete%20types%20and%20specific%20interfaces%20of%20all%20dependent%20objects%2C%20making%20the%20code%20inflexible%20and%20hard%20to%20extend.)</sup> 

The observer pattern provides a more flexible alternative by establishing a standard notification protocol:<sup>[4](https://en.wikipedia.org/wiki/Observer_pattern#:~:text=The%20observer%20pattern%20provides,when%20they%20are%20notified.)</sup> 
- Define `Subject` and `Observer` objects with standardized interfaces;
- When a subject changes state, all registered observers are notified and updated automatically;
- The subject manages its own state while also maintaining a list of observers and notifying them of state changes by calling their `update()` operation;
- The responsibility of observers is to register and unregister themselves with a subject (in order to be notified of state changes) and to update their state (to synchronize it with the subject's state) when they are notified.

This approach makes subject and observers loosely coupled through interface standardization. The subject only needs to know that observers implement the `update()` method—it has no knowledge of observers' concrete types or internal implementation details. Observers can be added and removed independently at run time.<sup>[5](https://en.wikipedia.org/wiki/Observer_pattern#:~:text=This%20approach%20makes,at%20run%20time.)</sup> 

Use cases in real world systems:<sup>[6](https://dev.to/zeeshanali0704/observer-design-pattern-in-java-complete-guide-1pe7#:~:text=%F0%9F%9B%A0%EF%B8%8F-,Use%20Cases%20in%20Real%20World%20Systems,Realtime%20dashboards%3A%20Reflecting%20live%20sensor%20or%20data%20stream%20updates.,-%E2%9C%85%20Advantages)</sup> 
- **GUI frameworks**. Buttons notify listeners when clicked;
- **Event handling systems**. Java’s AWT and Swing;
- **Messaging apps**. Update users when a new message arrives;
- **Stock market systems**. Notify brokers about stock price changes;
- **Realtime dashboards**. Reflecting live sensor or data stream updates.

## [Variations of the Observer Pattern](https://www.vogella.com/tutorials/DesignPatternObserver/article.html#observer_variations)
The Observer Pattern can be implemented in various ways, each tailored to specific use cases. Here are a few variations:
- **Push vs. Pull Model**
    - **Push Model**. The subject sends detailed information about its state to observers whenever a change occurs. This is useful when observers need specific data to react to changes;
    - **Pull Model**: Observers request the state of the subject after being notified of a change. This model is advantageous when observers may need to handle the state in a specific manner or when the subject’s state is large or complex;
- **Event Aggregator**. This variation centralizes event handling. Instead of having observers listen directly to subjects, they subscribe to a central event aggregator that manages the communication. This is useful in decoupling components in larger systems;
- **Reactive Programming**. In modern applications, especially those involving real-time data processing, reactive programming frameworks (like RxJava) extend the Observer Pattern. They provide more powerful abstractions for handling asynchronous data streams and event-driven programming.

## [Example](https://www.javaguides.net/2023/10/observer-design-pattern-in-kotlin.html#:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation steps:<sup>[7](https://www.javaguides.net/2023/10/observer-design-pattern-in-kotlin.html#:~:text=5.%20Implementation%20Steps,Observer%20and%20Subject.)</sup> 
- Define the `Observer` interface that outlines methods for being notified of updates;
- Define the `Subject` interface that allows observers to register, unregister, and is responsible for notifying them;
- Implement concrete classes for both `Observer` and `Subject`.

```
// Step 1: Observer interface
interface Observer {
    fun update(message: String)
}
// Step 2: Subject interface
interface Subject {
    fun registerObserver(o: Observer)
    fun unregisterObserver(o: Observer)
    fun notifyObservers()
}
// Step 3: Concrete implementations
class NewsAgency : Subject {
    private val observers: MutableList<Observer> = mutableListOf()
    private var news: String = ""
    override fun registerObserver(o: Observer) {
        observers.add(o)
    }
    override fun unregisterObserver(o: Observer) {
        observers.remove(o)
    }
    override fun notifyObservers() {
        for (observer in observers) {
            observer.update(news)
        }
    }
    fun postNews(newNews: String) {
        news = newNews
        notifyObservers()
    }
}
class NewsSubscriber(private val name: String) : Observer {
    override fun update(message: String) {
        println("$name received the news: $message")
    }
}
// Client Code
fun main() {
    val newsAgency = NewsAgency()
    val subscriber1 = NewsSubscriber("Subscriber 1")
    val subscriber2 = NewsSubscriber("Subscriber 2")
    newsAgency.registerObserver(subscriber1)
    newsAgency.registerObserver(subscriber2)
    newsAgency.postNews("Breaking News: Observer Pattern in Kotlin!")
}
```

Output:
```
Subscriber 1 received the news: Breaking News: Observer Pattern in Kotlin!
Subscriber 2 received the news: Breaking News: Observer Pattern in Kotlin!
```

Explanation:<sup>[8](https://www.javaguides.net/2023/10/observer-design-pattern-in-kotlin.html#:~:text=Explanation%3A,receive%20an%20update.)</sup> 
- The `Observer` and `Subject` interfaces define a contract for the pattern;
- `NewsAgency`, our concrete subject, maintains a list of observers and notifies them whenever new news is posted;
- `NewsSubscriber`, our concrete observer, defines how each observer should react when they receive an update.

# [Pros and Cons](https://dev.to/diegosilva13/understanding-the-observer-design-pattern-in-java-2lpc#:~:text=in%20its%20state.-,Pros%20and%20Cons,-Pros)
Pros:
- **Decoupling**. The pattern promotes loose coupling between the subject and observers, allowing them to evolve independently;
- **Reactivity**. Enables observers to automatically receive updates when the subject's state changes, supporting the development of reactive systems;
- **Extensibility**. New observers can be added without modifying the existing subject or other observers' code.

Cons:
- **Complexity**. Can introduce additional complexity, especially in systems with many observers and frequent events;
- **Observer Management**. Managing the lifecycle of observers, including registration and removal, can be complex;
- **Potential Performance Issues**. Notifying a large number of observers can impact performance, especially if the update methods are complex.

# Links
[Observer Design Pattern](https://www.geeksforgeeks.org/system-design/observer-pattern-set-1-introduction/)

[Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern)

[Observer Design Pattern in Java – Complete Guide](https://dev.to/zeeshanali0704/observer-design-pattern-in-java-complete-guide-1pe7)

[Observer Design Pattern in Java - Tutorial](https://www.vogella.com/tutorials/DesignPatternObserver/article.html)

[Observer Design Pattern in Kotlin](https://www.javaguides.net/2023/10/observer-design-pattern-in-kotlin.html)

[Understanding the Observer Design Pattern in Java](https://dev.to/diegosilva13/understanding-the-observer-design-pattern-in-java-2lpc)

# Further reading
[Observer Design Pattern](https://sourcemaking.com/design_patterns/observer)

[Observer](https://refactoring.guru/design-patterns/observer)

[Observer Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/observer-software-pattern-kotlin-example)

# Nex questions
[What is memory leak?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20is%20memory%20leak.md)
