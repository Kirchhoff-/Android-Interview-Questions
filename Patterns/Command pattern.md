# Command pattern
Command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

Using the Command design pattern can solve these problems:
- Coupling the invoker of a request to a particular request should be avoided. That is, hard-wired requests should be avoided.
- It should be possible to configure an object (that invokes a request) with a request.

Implementing (hard-wiring) a request directly into a class is inflexible because it couples the class to a particular request at compile-time, which makes it impossible to specify a request at run-time.
Using the Command design pattern describes the following solution:
- Define separate (command) objects that encapsulate a request.
- A class delegates a request to a command object instead of implementing a particular request directly.

This enables one to configure a class with a command object that is used to perform a request. The class is no longer coupled to a particular request and has no knowledge (is independent) of how the request is carried out.

## Example
Suppose we have interface `Command`:
```
public interface Command {
    public void execute();
}
```

Also we have class `TV`, which is known how to run command:
```
public final class TV {

    private Command lastCommand = null;

    public void addCommand(Command command){
        lastCommand = command;
    }

    public void executeCommand() {
        lastCommand.execute();
    }
}
```

There are few implementation of `Command` interface:
```
public class TurnOnCommand implements Command {
    
    @Override
    public void execute() {
        System.out.println("Turn on device");
    }
}
```

```
public class TurnOffCommand implements Command {

    @Override
    public void execute() {
        System.out.println("Turn off device");
    }
}
```

Example of usage:
```
public class CommandDemo {

    public void commandExample() {
        TV tv = new TV();
        Command turnOn = new TurnOnCommand();
        Command turnOff = new TurnOffCommand();

        tv.addCommand(turnOn);
        tv.executeCommand();

        tv.addCommand(turnOff);
        tv.executeCommand();
    }
}
```

Output:

```
Turn on device
Turn off device
```

Advantages:
- Makes our code extensible as we can add new commands without changing existing code.
- Reduces coupling the invoker and receiver of a command.

Disadvantages:
- Increase in the number of classes for each individual command

## Links
https://en.wikipedia.org/wiki/Command_pattern  
https://www.geeksforgeeks.org/command-pattern/  
https://www.baeldung.com/java-command-pattern  
