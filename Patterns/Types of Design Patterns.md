# Design Patterns
Design patterns is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. Rather, it is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system.

Object-oriented design patterns typically show relationships and interactions between classes or objects, without specifying the final application classes or objects that are involved. Patterns that imply mutable state may be unsuited for functional programming languages, some patterns can be rendered unnecessary in languages that have built-in support for solving the problem they are trying to solve, and object-oriented patterns are not necessarily suitable for non-object-oriented languages.

In addition, patterns allow developers to communicate using well-known, well understood names for software interactions. Common design patterns can be improved over time, making them more robust than ad-hoc designs.

Design patterns are divided into three fundamental groups:
- Behavioral
- Creational
- Structural

## Behavioral 
Behavioral patterns describe interactions between objects and focus on how objects communicate with each other. They can reduce complex flow charts to mere interconnections between objects of various classes. Behavioral patterns are also used to make the algorithm that a class uses simply another parameter that is adjustable at runtime.
Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects. Behavioral patterns describe not just patterns of objects or classes but also the patterns of communication between them. These patterns characterize complex control flow that is difficult to follow at run-time. They shift your focus away from the flow of control to let you concentrate just on the way objects are interconnected. Behavioral class patterns use inheritance to distribute behavior between classes.

Patterns: 
- **Chain of responsibility** - A way of passing a request between a chain of objects
- **Command** - Encapsulate a command request as an object
- **Interpreter** - A way to include language elements in a program
- **Iterator** - Sequentially access the elements of a collection
- **Mediator** - Defines simplified communication between classes
- **Memento** - Capture and restore an object's internal state
- **Observer** - A way of notifying change to a number of classes
- **State** - Alter an object's behavior when its state changes
- **Strategy** - Encapsulates an algorithm inside a class
- **Template method** - Defer the exact steps of an algorithm to a subclass
- **Visitor** - Defines a new operation to a class without change

## Creational
Creational patterns are used to create objects for a suitable class that serves as a solution for a problem. Generally when instances of several different classes are available. They are particularly useful when you are taking advantage of polymorphism and need to choose between different classes at runtime rather than compile time.

Creational patterns support the creation of objects in a system. Creational patterns allow objects to be created in a system without having to identify a specific class type in the code, so you do not have to write large, complex code to instantiate an object. It does this by having the subclass of the class create the objects. However, this can limit the type or number of objects that can be created within a system.

Patterns: 
- **Abstract Factory** - Creates an instance of several families of classes
- **Builder** - Separates object construction from its representation
- **Factory Method** - Creates an instance of several derived classes
- **Object Pool** - Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
- **Prototype** - A fully initialized instance to be copied or cloned
- **Singleton** - A class of which only a single instance can exist

## Structural
Structural patterns form larger structures from individual parts, generally of different classes. Structural patterns vary a great deal depending on what sort of structure is being created for what purpose.

Structural patterns are concerned with how classes and objects are composed to form larger structures. Structural class patterns use inheritance to compose interfaces or implementations. As a simple example, consider how multiple inheritance mixes two or more classes into one. The result is a class that combines the properties of its parent classes. This pattern is particularly useful for making independently developed class libraries work together.

Patterns: 
- **Adapter** - Match interfaces of different classes
- **Bridge** - Separates an object’s interface from its implementation
- **Composite** - A tree structure of simple and composite objects
- **Decorator** - Add responsibilities to objects dynamically
- **Facade** - A single class that represents an entire subsystem
- **Flyweight** - A fine-grained instance used for efficient sharing
- **Proxy** - An object representing another object

## Links
https://en.wikipedia.org/wiki/Software_design_pattern  
https://www.gofpatterns.com/design-patterns/module2/three-types-design-patterns.php  
https://www.freecodecamp.org/news/the-basic-design-patterns-all-developers-need-to-know/  
https://sourcemaking.com/design_patterns  
