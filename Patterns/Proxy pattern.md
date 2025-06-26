# Proxy pattern
The Proxy Design Pattern a structural design pattern is a way to use a placeholder object to control access to another object. Instead of interacting directly with the main object, the client talks to the proxy, which then manages the interaction. This is useful for things like controlling access, delaying object creation until it's needed (lazy initialization), logging, or adding security checks.<sup>[1](https://www.geeksforgeeks.org/system-design/proxy-design-pattern/#:~:text=The%20Proxy%20Design,adding%20security%20checks.)</sup>

What problems can the Proxy design pattern solve?<sup>[2](https://en.wikipedia.org/wiki/Proxy_pattern#:~:text=What%20problems%20can%20the%20Proxy%20design%20pattern%20solve%3F%5B,Additional%20functionality%20should%20be%20provided%20when%20accessing%20an%20object.)</sup>
- The access to an object should be controlled;
- Additional functionality should be provided when accessing an object.

What solution does the Proxy design pattern describe?<sup>[3](https://en.wikipedia.org/wiki/Proxy_pattern#:~:text=What%20solution%20does%20the%20Proxy%20design%20pattern%20describe%3F%5B,implements%20additional%20functionality%20to%20control%20the%20access%20to%20this%20subject.)</sup>

Define a separate `Proxy` object that:
- can be used as a substitute for another object (`Subject`);
- implements additional functionality to control the access to this subject.

This makes it possible to work through a `Proxy` object to perform additional functionality when accessing a subject, like checking the access rights of clients accessing a sensitive object.

To act as a substitute for a subject, a proxy must implement the `Subject` interface. Clients can't tell whether they work with a subject or its proxy.<sup>[4](https://en.wikipedia.org/wiki/Proxy_pattern#:~:text=This%20makes%20it,or%20its%20proxy.)</sup>

The main use cases are:<sup>[5](https://fugisawa.com/articles/kotlin-design-patterns-simplifying-the-proxy-pattern/#:~:text=The%20main%20use,to%20enhance%20performance.)</sup>
- **Lazy initialization (Virtual Proxy)**. Delays the creation of expensive objects until necessary, saving resources;
- **Access control (Protection Proxy)**. Manages access to an object, allowing or denying operations based on permission checks;
- **Remote object access (Remote Proxy)**. Represents an object in a different location, handling communication in networked applications;
- **Logging requests (Logging Proxy)**. Records operations on an object, useful for debugging and monitoring;
- **Caching**. Stores results of operations, returning cached data for repeated requests to enhance performance.

## [Example](https://www.javaguides.net/2023/10/proxy-design-pattern-in-kotlin.html#:~:text=Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[6](https://www.javaguides.net/2023/10/proxy-design-pattern-in-kotlin.html#:~:text=5.%20Implementation%20Steps,the%20real%20object.)</sup>
- Create an interface that both the real object and the proxy will implement;
- Implement the real object that will perform the main operations;
- Implement the proxy object that will add additional behaviors and control access to the real object.

```
// Step 1: Interface
interface Database {
    fun query(dbQuery: String): String
}
// Step 2: Real Object
class RealDatabase : Database {
    override fun query(dbQuery: String): String {
        // Simulating a database operation
        return "Executing query: $dbQuery"
    }
}
// Step 3: Proxy Object
class ProxyDatabase : Database {
    private val realDatabase = RealDatabase()
    private val restrictedQueries = listOf("DROP", "DELETE")
    override fun query(dbQuery: String): String {
        if (restrictedQueries.any { dbQuery.contains(it, ignoreCase = true) }) {
            return "Query not allowed!"
        }
        // Logging operation
        println("Logging: $dbQuery")
        return realDatabase.query(dbQuery)
    }
}
fun main() {
    val database = ProxyDatabase()
    println(database.query("SELECT * FROM users"))
    println(database.query("DROP TABLE users"))
}
```

Output:
```
Logging: SELECT * FROM users
Executing query: SELECT * FROM users
Query not allowed!
```

Explanation:<sup>[7](https://www.javaguides.net/2023/10/proxy-design-pattern-in-kotlin.html#:~:text=Explanation%3A,and%20not%20executed.)</sup>
- `Database` is the common interface for `RealDatabase` and `ProxyDatabase`;
- `RealDatabase` performs the real operations;
- `ProxyDatabase` controls access to `RealDatabase`. It adds a layer of logging and restricts certain queries;
- In the `main` function, when using `ProxyDatabase`, the `SELECT` query is logged and executed, but the `DROP` query is restricted and not executed.

## Pros and Cons
Pros:<sup>[8](https://medium.com/@samibel/understanding-the-proxy-design-pattern-in-kotlin-23fee0fe8aac#:~:text=Pros%20and%20Cons-,Pros,transparency%20ensures%20that%20existing%20codebases%20can%20adopt%20proxies%20without%20extensive%20modifications.,-Cons)</sup>
- **Separation of Concerns**. The Proxy pattern allows for the separation of the primary business logic from the auxiliary responsibilities (like caching, access control, or lazy initialization). This enhances code maintainability and clarity;
- **Improved Performance and Efficiency**. By using techniques like lazy initialization and caching, the Proxy pattern can significantly reduce the cost of resource-intensive operations, leading to better performance and lower resource consumption;
- **Access Control**. Proxies can control the access to an object, which is particularly useful in scenarios where certain conditions must be met before the access is granted (e.g., checking permissions or authenticating a user);
- **Transparent to the Client**. The use of a proxy does not change the way the client interacts with the actual object. This transparency ensures that existing codebases can adopt proxies without extensive modifications.

Cons:<sup>[9](https://medium.com/@samibel/understanding-the-proxy-design-pattern-in-kotlin-23fee0fe8aac#:~:text=without%20extensive%20modifications.-,Cons,lead%20to%20more%20complex%20test%20setups%20and%20potentially%20longer%20testing%20cycles.,-In%20conclusion%2C%20while)</sup>
- **Additional Complexity**. Introducing proxies into your system can add an extra layer of complexity, potentially making the system harder to understand and maintain. This complexity might not always be justified, especially for simple scenarios;
- **Potential Performance Overhead**. While proxies can improve performance by caching or lazy loading, they also introduce an extra layer of indirection which might slightly impact performance in scenarios where direct access would have been more efficient;
- **Design Complexity**. Properly implementing the Proxy pattern requires a good understanding of the system’s design and the specific use case it aims to solve. Incorrectly implemented proxies can lead to issues like memory leaks (in the case of improper caching) or unnecessary delays (in the case of mismanaged lazy loading);
- **Testing Overhead**. Testing an application that makes extensive use of proxies can be more challenging, as unit tests might need to account for the behavior of the proxy as well as the actual object. This can lead to more complex test setups and potentially longer testing cycles.

# Links
[Proxy Design Pattern](https://www.geeksforgeeks.org/system-design/proxy-design-pattern/)

[Proxy pattern](https://en.wikipedia.org/wiki/Proxy_pattern)

[Kotlin Design Patterns: Simplifying the Proxy Pattern](https://fugisawa.com/articles/kotlin-design-patterns-simplifying-the-proxy-pattern/)

[Proxy Design Pattern in Kotlin](https://www.javaguides.net/2023/10/proxy-design-pattern-in-kotlin.html)

[Understanding the Proxy design Pattern in Kotlin](https://medium.com/@samibel/understanding-the-proxy-design-pattern-in-kotlin-23fee0fe8aac)

# Further reading
[Proxy Design Pattern](https://sourcemaking.com/design_patterns/proxy)

[Proxy](https://refactoring.guru/design-patterns/proxy)

[Proxy Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/proxy-software-pattern-kotlin-example)
