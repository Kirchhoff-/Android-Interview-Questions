# Command pattern
The Command Pattern is a behavioral design pattern that turns a request into a stand-alone object that contains all the information about the request. This transformation allows you to parameterize methods with different requests, queue requests, and log their execution, among other things. At its core, it decouples the object that initiates an action from the object that performs the action.<sup>[1](https://codesignal.com/learn/courses/behavioral-design-patterns-2/lessons/command-pattern-in-kotlin#:~:text=The%20Command%20Pattern%20is,that%20performs%20the%20action.)</sup>

Using the command design pattern can solve these problems:
- Coupling the invoker of a request to a particular request should be avoided. That is, hard-wired requests should be avoided;
- It should be possible to configure an object (that invokes a request) with a request.

Implementing (hard-wiring) a request directly into a class is inflexible because it couples the class to a particular request at compile-time, which makes it impossible to specify a request at run-time.

Using the command design pattern describes the following solution:
- Define separate (command) objects that encapsulate a request;
- A class delegates a request to a command object instead of implementing a particular request directly.

This enables one to configure a class with a command object that is used to perform a request. The class is no longer coupled to a particular request and has no knowledge (is independent) of how the request is carried out.<sup>[2](https://en.wikipedia.org/wiki/Command_pattern#:~:text=Using%20the%20command,is%20carried%20out.):

The Command Pattern is especially useful for<sup>[3](https://codesignal.com/learn/courses/behavioral-design-patterns-2/lessons/command-pattern-in-kotlin#:~:text=The%20Command%20Pattern%20is%20especially,for%20debugging%20and%20auditing%20purposes.):
- **Decoupling**. It decouples the object that requests an operation (invoker) from the one that actually performs the operation (receiver);
- **Reusability**. Commands can be reused and combined in complex scenarios, enhancing code maintainability and flexibility;
- **History and Undo**. It allows for the addition of history and undo features, crucial in applications like text editors, where users may want to revert actions;
- **Logging**. Commands can be logged, enabling actions to be recorded and replayed. This is useful for debugging and auditing purposes.

## [Real Life Examples of Command Design Pattern](https://medium.com/@akshatsharma0610/command-design-pattern-in-java-1b8bbb699ff7#:~:text=Real%20Life%20Examples%20of%20Command%20Design%20Pattern)
- **GUI applications**. GUI frameworks often utilize the Command pattern extensively. For instance, when you click a button on a graphical user interface, a command object is typically created and executed to perform the corresponding action. This allows the GUI framework to decouple the event trigger (button click) from the specific operation that needs to be performed, providing flexibility and extensibility;
- **Text editors**. Text editors often employ the Command pattern to implement undo/redo functionality. Each user action, such as typing a character, deleting a line, or formatting text, can be encapsulated as a command object. The text editor maintains a history of executed commands, enabling the ability to undo and redo operations by executing the respective command objects in reverse order or forward order;
- **Remote controls**. The Command pattern is commonly used in remote control systems, such as TV remotes or home automation systems. Each button on the remote control is associated with a specific command object. When a button is pressed, the corresponding command object is executed, triggering the desired action on the controlled device (e.g., turning on/off lights, changing channels, adjusting volume);
- **Job scheduling**. Command objects can be utilized in job scheduling systems. Each job or task to be executed is encapsulated as a command object, which contains all the necessary information and logic to perform the task. The job scheduler maintains a queue of command objects and executes them based on predefined criteria, such as priority, time, or dependencies;
- **Transaction management**. In database systems, the Command pattern can be employed to manage transactions. Each database operation, such as inserting a record, updating data, or deleting records, can be encapsulated as a command object. The transaction manager maintains a list of executed commands and provides methods to commit or rollback the transaction by executing or undoing the respective command objects.

## [Example](https://www.javaguides.net/2023/10/command-design-pattern-in-kotlin.html#:~:text=Implementation%20Steps)
Implementation steps:
1. Define a command interface with an execute method;
2. Create one or more concrete classes that implement this command interface, where each class represents a different operation;
3. Define an invoker class that asks the command to carry out the request.

```
// Step 1: Command Interface
interface Command {
    fun execute()
}
// Step 2: Concrete Command Classes
class LightOnCommand(private val light: Light) : Command {
    override fun execute() {
        light.turnOn()
    }
}
class LightOffCommand(private val light: Light) : Command {
    override fun execute() {
        light.turnOff()
    }
}
// Receiver Class
class Light {
    fun turnOn() {
        println("Light is ON")
    }
    fun turnOff() {
        println("Light is OFF")
    }
}
// Step 3: Invoker
class RemoteControl(private val command: Command) {
    fun pressButton() {
        command.execute()
    }
}
// Test the implementation
fun main() {
    val light = Light()
    val lightOnCommand = LightOnCommand(light)
    val lightOffCommand = LightOffCommand(light)
    val remote = RemoteControl(lightOnCommand)
    remote.pressButton()
    val remote2 = RemoteControl(lightOffCommand)
    remote2.pressButton()
}
```

Output:
```
Light is ON
Light is OFF
```

[Explanation](https://www.javaguides.net/2023/10/command-design-pattern-in-kotlin.html#:~:text=Light%20is%20OFF-,Explanation%3A,-1.%20The%20Command):
1. The `Command` interface declares a method named `execute`;
2. `LightOnCommand` and `LightOffCommand` are concrete implementations of the `Command` interface. They encapsulate the action and the receiver;
3. The `RemoteControl` class (invoker) allows you to execute a command when its button is pressed;
4. In the `main` function, a light is turned on and then off using the command pattern.

# Links
[Command Pattern in Kotlin](https://codesignal.com/learn/courses/behavioral-design-patterns-2/lessons/command-pattern-in-kotlin)

[Command pattern](https://en.wikipedia.org/wiki/Command_pattern)

[Command Design Pattern in Java](https://medium.com/@akshatsharma0610/command-design-pattern-in-java-1b8bbb699ff7)

[Command Design Pattern in Kotlin](https://www.javaguides.net/2023/10/command-design-pattern-in-kotlin.html)

# Further reading
[Command](https://refactoring.guru/design-patterns/command)

[Command Pattern in Kotlin](https://swiderski.tech/kotlin-command-pattern/)

[Command Design Pattern in Java](https://medium.com/@neerukapoor/command-design-pattern-in-java-7d06dfdd31)

[Understanding the Command Design Pattern and Its Role in Android Development](https://medium.com/kotlin-android-chronicle/understanding-the-command-design-pattern-and-its-role-in-android-development-9eb1dc00350b)
