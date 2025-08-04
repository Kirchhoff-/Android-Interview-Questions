# Chain of Responsibility
Chain of Responsibility is a design pattern in which a request is passed sequentially through a chain of potential handlers until one of them handles the request. Each handler in the chain has a chance to either process the request or pass it to the next handler. The client (the code that issues the request) doesn’t need to know which handler will actually deal with the request — it simply sends it into the chain. This decouples the client from the specific receiver that performs the action, following the Open/Closed Principle by allowing new handlers to be added without modifying existing code.<sup>[1](https://maxim-gorin.medium.com/stop-hardcoding-logic-use-the-chain-of-responsibility-instead-62146c9cf93a#:~:text=Chain%20of%20Responsibility%20is,without%20modifying%20existing%20code.)</sup>

What problems can the Chain of Responsibility design pattern solve?<sup>[2](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern#:~:text=test%2C%20and%20reuse.-,What%20problems%20can%20the%20Chain%20of%20Responsibility%20design%20pattern%20solve%3F,be%20possible%20that%20more%20than%20one%20receiver%20can%20handle%20a%20request.,-Implementing%20a%20request)</sup>
- Coupling the sender of a request to its receiver should be avoided;
- It should be possible that more than one receiver can handle a request.

Implementing a request directly within the class that sends the request is inflexible because it couples the class to a particular receiver and makes it impossible to support multiple receivers.<sup>[3](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern#:~:text=Implementing%20a%20request%20directly%20within%20the%20class%20that%20sends%20the%20request%20is%20inflexible%20because%20it%20couples%20the%20class%20to%20a%20particular%20receiver%20and%20makes%20it%20impossible%20to%20support%20multiple%20receivers.)</sup>

What solution does the Chain of Responsibility design pattern describe?<sup>[4](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern#:~:text=%5B3%5D-,What%20solution%20does%20the%20Chain%20of%20Responsibility%20design%20pattern%20describe%3F,forward%20it%20to%20the%20next%20receiver%20on%20the%20chain%20(if%20any).,-This%20enables%20us)</sup>
- Define a chain of receiver objects having the responsibility, depending on run-time conditions, to either handle a request or forward it to the next receiver on the chain (if any).

This enables us to send a request to a chain of receivers without having to know which one handles the request. The request gets passed along the chain until a receiver handles the request. The sender of a request is no longer coupled to a particular receiver.<sup>[5](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern#:~:text=This%20enables%20us%20to%20send%20a%20request%20to%20a%20chain%20of%20receivers%20without%20having%20to%20know%20which%20one%20handles%20the%20request.%20The%20request%20gets%20passed%20along%20the%20chain%20until%20a%20receiver%20handles%20the%20request.%20The%20sender%20of%20a%20request%20is%20no%20longer%20coupled%20to%20a%20particular%20receiver.)</sup>

You might consider using Chain of Responsibility when:<sup>[6](https://maxim-gorin.medium.com/stop-hardcoding-logic-use-the-chain-of-responsibility-instead-62146c9cf93a#:~:text=You%20might%20consider,requests%20are%20handled.)</sup>
- **You have multiple handlers for a type of request, and which one handles it may depend on runtime conditions**. For example, an event in a GUI might be handled by the control that’s clicked, or if that doesn’t handle it, by its parent container, and so on up the hierarchy. CoR is frequently used in UI frameworks for event propagation: the event bubbles through a chain of UI components until one consumes it;
- **You want to avoid monolithic conditional logic**. If you find yourself writing a big `if/else if/else if/...` chain or a `switch` (`when` in Kotlin) statement to decide what to do with a request, that’s a code smell CoR can address. The pattern distributes those condition checks into separate handler classes. This not only makes the code cleaner, but also makes it easy to add new conditions (handlers) without modifying the existing dispatching logic – adhering to the Open/Closed Principle;
- **You need flexible chains that can be extended or reordered**. Because handlers are separate objects, you can compose different chains easily. For instance, you might set up a chain of content filters (profanity filter, spam filter, etc.) for messages. You could enable or disable certain filters by adding or removing them from the chain without touching the code of the others. The chain can even be configured at runtime (e.g., reading handler configuration from a file), allowing dynamic reconfiguration of how requests are handled.

## [Example](https://www.javaguides.net/2023/10/chain-of-responsibility-design-pattern-in-kotlin.html#:~:text=Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[7](https://www.javaguides.net/2023/10/chain-of-responsibility-design-pattern-in-kotlin.html#:~:text=5.%20Implementation%20Steps,through%20the%20chain.)</sup>
- Define the handler interface or abstract class with a method to set the next handler and another to handle the request;
- Create concrete handler classes that implement the handler interface or extend the abstract class;
- Build the chain by connecting handlers, and send the request through the chain.

```
// Step 1: Handler Interface
interface Handler {
    var nextHandler: Handler?
    fun handleRequest(request: String): Boolean
}
// Step 2: Concrete Handlers
class FirstHandler : Handler {
    override var nextHandler: Handler? = null
    override fun handleRequest(request: String): Boolean {
        if (request == "Request1") {
            println("FirstHandler handled $request")
            return true
        }
        return nextHandler?.handleRequest(request) ?: false
    }
}
class SecondHandler : Handler {
    override var nextHandler: Handler? = null
    override fun handleRequest(request: String): Boolean {
        if (request == "Request2") {
            println("SecondHandler handled $request")
            return true
        }
        return nextHandler?.handleRequest(request) ?: false
    }
}
// Step 3: Building the chain and testing
fun main() {
    val firstHandler = FirstHandler()
    val secondHandler = SecondHandler()
    firstHandler.nextHandler = secondHandler
    listOf("Request1", "Request2", "Request3").forEach {
        if (!firstHandler.handleRequest(it)) {
            println("$it was not handled")
        }
    }
}
```

Output:
```
FirstHandler handled Request1
SecondHandler handled Request2
Request3 was not handled
```

Explanation:<sup>[8](https://www.javaguides.net/2023/10/chain-of-responsibility-design-pattern-in-kotlin.html#:~:text=Explanation%3A,through%20the%20chain.)</sup>
- The `Handler` interface declares methods for handling requests and setting the next handler in the chain;
- `FirstHandler` and `SecondHandler` are concrete handlers that check if they can handle a request. If not, they pass it to the next handler;
- In the `main` function, the handlers are linked to form a chain and then various requests are sent through the chain.

## Pros and Cons
Pros:<sup>[9](https://www.geeksforgeeks.org/system-design/chain-responsibility-design-pattern/#:~:text=The%20pattern%20makes,the%20processing%20logic.)</sup>
- The pattern makes enables sending a request to a series of possible recipients without having to worry about which object will handle it in the end. This lessens the reliance between items;
- New handlers can be easily added or existing ones can be modified without affecting the client code. This promotes flexibility and extensibility within the system;
- The sequence and order of handling requests can be changed dynamically during runtime, which allows adjustment of the processing logic as per the requirements;
- It simplifies the interaction between the sender and receiver objects, as the sender does not need to know about the processing logic.

Cons:<sup>[10](https://www.geeksforgeeks.org/system-design/chain-responsibility-design-pattern/#:~:text=The%20chain%20should,modified%20at%20runtime.)</sup>
- The chain should be implemented correctly otherwise there is a chance that some requests might not get handled at all, which leads to unexpected behavior in the application;
- The request will go through several handlers in the chain if it is lengthy and complicated, which could cause performance overhead. The processing logic of each handler has an effect on the system’s overall performance;
- The fact that the chain has several handlers can make debugging more difficult. Tracking the progression of a request and determining which handler is in charge of handling it can be difficult;
- It may become more difficult to manage and maintain the chain of responsibility if the chain is dynamically modified at runtime.

# Links
[Stop Hardcoding Logic: Use the Chain of Responsibility Instead](https://maxim-gorin.medium.com/stop-hardcoding-logic-use-the-chain-of-responsibility-instead-62146c9cf93a)

[Chain-of-responsibility pattern](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern)

[Chain of Responsibility Design Pattern in Kotlin](https://www.javaguides.net/2023/10/chain-of-responsibility-design-pattern-in-kotlin.html)

[Chain of Responsibility Design Pattern](https://www.geeksforgeeks.org/system-design/chain-responsibility-design-pattern/)

# Further reading
[Chain of Responsibility](https://sourcemaking.com/design_patterns/chain_of_responsibility)

[Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)

[Chain of Responsibility Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/chain-of-responsibility-software-pattern-kotlin-example)
