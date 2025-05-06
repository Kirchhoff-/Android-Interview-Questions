# Composite pattern
The Composite Design Pattern is a structural design pattern that lets you compose objects into tree-like structures to represent part-whole hierarchies. It allows clients to treat individual objects and compositions of objects uniformly. In other words, whether dealing with a single object or a group of objects (composite), clients can use them interchangeably.<sup>[1](https://www.geeksforgeeks.org/composite-design-pattern-in-java/#:~:text=The%20Composite%20Design,use%20them%20interchangeably.)</sup>

The composite design pattern solves problems like:<sup>[2](https://en.wikipedia.org/wiki/Composite_pattern#:~:text=Problems%20the%20Composite%20design%20pattern%20can%20solve%5B,Represent%20a%20part%2Dwhole%20hierarchy%20as%20tree%20structure.)</sup>
- Represent a part-whole hierarchy so that clients can treat part and whole objects uniformly;
- Represent a part-whole hierarchy as tree structure.

When defining (1) `Part` objects and (2) `Whole` objects that act as containers for `Part` objects, clients must treat them separately, which complicates client code.<sup>[3](https://en.wikipedia.org/wiki/Composite_pattern#:~:text=When%20defining%20(1)%20Part%20objects%20and%20(2)%20Whole%20objects%20that%20act%20as%20containers%20for%20Part%20objects%2C%20clients%20must%20treat%20them%20separately%2C%20which%20complicates%20client%20code.)</sup>

Solutions the Composite design pattern describes:<sup>[4](https://en.wikipedia.org/wiki/Composite_pattern#:~:text=Solutions%20the%20Composite%20design%20pattern%20describes%5B,objects%20forward%20requests%20to%20their%20child%20components.)</sup>
- Define a unified `Component` interface for part (`Leaf`) objects and whole (`Composite`) objects;
- Individual `Leaf` objects implement the `Component` interface directly, and `Composite` objects forward requests to their child components.

This enables clients to work through the `Component` interface to treat `Leaf` and `Composite` objects uniformly: `Leaf` objects perform a request directly, and `Composite` objects forward the request to their child components recursively downwards the tree structure. This makes client classes easier to implement, change, test, and reuse.<sup>[5](https://en.wikipedia.org/wiki/Composite_pattern#:~:text=This%20enables%20clients,test%2C%20and%20reuse.)</sup>

## When to use
Composite should be used when clients ignore the difference between compositions of objects and individual objects. If programmers find that they are using multiple objects in the same way, and often have nearly identical code to handle each of them, then composite is a good choice; it is less complex in this situation to treat primitives and composites as homogeneous.<sup>[6](https://en.wikipedia.org/wiki/Composite_pattern#:~:text=Composite%20should%20be,composites%20as%20homogeneous.)</sup>

Other reasons to use this pattern:<sup>[7](https://dev.to/syridit118/understanding-the-composite-design-pattern-a-comprehensive-guide-with-real-world-applications-4855#:~:text=When%20to%20Use,of%20objects%20uniformly.)</sup>
- **Tree-like structures**. When the system has a natural hierarchy where objects can be composed of other objects, such as graphical shapes, file systems, UI components, and organizational structures;
- **Recursive structures**. When objects are made up of smaller objects of the same type (e.g., directories containing files and other directories);
- **Simplifying client code**. When you want the client code to treat individual objects and compositions of objects uniformly.

## [Example](https://www.javaguides.net/2023/10/composite-design-pattern-in-kotlin.html#google_vignette:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation steps:<sup>[8](https://www.javaguides.net/2023/10/composite-design-pattern-in-kotlin.html#google_vignette:~:text=5.%20Implementation%20Steps,and%20have%20children.)</sup>
- Define a component interface that makes individual and composite objects uniform;
- Create leaf objects that implement the component;
- Create composite objects that also implement the component and have children.

```
// Component
interface Employee {
    fun showDetails(): String
}
// Leaf
class Developer(private val name: String, private val position: String) : Employee {
    override fun showDetails() = "Developer: $name, Position: $position"
}
// Composite
class Manager(private val name: String, private val position: String) : Employee {
    private val employees: MutableList<Employee> = mutableListOf()
    fun addEmployee(employee: Employee) {
        employees.add(employee)
    }
    fun removeEmployee(employee: Employee) {
        employees.remove(employee)
    }
    override fun showDetails(): String {
        val employeeDetails = employees.joinToString("\n") { it.showDetails() }
        return "Manager: $name, Position: $position\n$employeeDetails"
    }
}
fun main() {
    val dev1 = Developer("John Doe", "Frontend Developer")
    val dev2 = Developer("Jane Smith", "Backend Developer")
    val manager = Manager("Ella White", "Tech Lead")
    manager.addEmployee(dev1)
    manager.addEmployee(dev2)
    println(manager.showDetails())
}
```

Output:
```
Manager: Ella White, Position: Tech Lead
Developer: John Doe, Position: Frontend Developer
Developer: Jane Smith, Position: Backend Developer
```

Explanation:<sup>[9](https://www.javaguides.net/2023/10/composite-design-pattern-in-kotlin.html#google_vignette:~:text=Explanation%3A,their%20details%20uniformly.)</sup>
- `Employee` is the component interface that provides a uniform method `showDetails`;
- `Developer` is a leaf that implements the `Employee` interface;
- `Manager` is a composite that contains a list of `Employees` and implements the same interface. It can add or remove employees;
- In the `main` function, we created a manager with two developers reporting to her and displaying their details uniformly.

# [Pros and Cons](https://www.adityatechinsights.com/composite-design-pattern-java-explained#heading-pros-and-cons)
Pros:
- **Flexibility**. The composite pattern makes it easy to add new types of components to the hierarchy without having to change the existing code;
- **Simplified client code**. Clients can treat individual objects and groups of objects in the same way, which simplifies the code and makes it easier to maintain;
- **Improved scalability**. The composite pattern allows you to build complex hierarchical structures that can be easily scaled and modified as needed;
- **Reduced coupling**. The pattern reduces the coupling between the client and the objects in the hierarchy, making it easier to modify and maintain the code.

Cons:
- **Increased complexity**. The pattern can make the code more complex and harder to understand, especially when dealing with deep and complex hierarchies;
- **Performance overhead**. The pattern can result in some performance overhead, especially when dealing with large hierarchies, due to the extra layers of abstraction and indirection;
- **Limited functionality**. The pattern is best suited for hierarchies of similar objects, and may not be suitable for other types of structures.

# Links
[Composite Design Pattern in Java](https://www.geeksforgeeks.org/composite-design-pattern-in-java/)

[Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern)

[Understanding the Composite Design Pattern: A Comprehensive Guide with Real-World Applications](https://dev.to/syridit118/understanding-the-composite-design-pattern-a-comprehensive-guide-with-real-world-applications-4855)

[Composite Design Pattern in Kotlin](https://www.javaguides.net/2023/10/composite-design-pattern-in-kotlin.html#google_vignette)

[composite pattern - Java - Explained](https://www.adityatechinsights.com/composite-design-pattern-java-explained)

# Further reading
[Composite Design Pattern](https://sourcemaking.com/design_patterns/composite)

[Composite](https://refactoring.guru/design-patterns/composite)

[Composite Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/composite-software-pattern-kotlin-example)
