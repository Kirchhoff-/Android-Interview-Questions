# Template method
The template method is a method in a superclass, usually an abstract superclass, and defines the skeleton of an operation in terms of a number of high-level steps. These steps are themselves implemented by additional *helper methods* in the same class as the template method.

The *helper methods* may be either abstract methods, for which case subclasses are required to provide concrete implementations, or hook methods, which have empty bodies in the superclass. Subclasses can (but are not required to) customize the operation by overriding the hook methods. The intent of the template method is to define the overall structure of the operation, while allowing subclasses to refine, or redefine, certain steps

This pattern has two main parts:
- The "template method" is implemented as a method in a base class (usually an abstract class). This method contains code for the parts of the overall algorithm that are invariant. The template ensures that the overarching algorithm is always followed. In the template method, portions of the algorithm that may vary are implemented by sending self messages that request the execution of additional helper methods. In the base class, these helper methods are given a default implementation, or none at all (that is, they may be abstract methods).
- Subclasses of the base class "fill in" the empty or "variant" parts of the "template" with specific algorithms that vary from one subclass to another. It is important that subclasses do not override the template method itself.

At run-time, the algorithm represented by the template method is executed by sending the template message to an instance of one of the concrete subclasses. Through inheritance, the template method in the base class starts to execute. When the template method sends a message to self requesting one of the helper methods, the message will be received by the concrete sub-instance. If the helper method has been overridden, the overriding implementation in the sub-instance will execute; if it has not been overridden, the inherited implementation in the base class will execute. This mechanism ensures that the overall algorithm follows the same steps every time, while allowing the details of some steps to depend on which instance received the original request to execute the algorithm.

The template method is used in frameworks, where each implements the invariant parts of a domain's architecture, while providing hook methods for customization. The template method is used for the following reasons:

- It lets subclasses implement varying behavior (through overriding of the hook methods).
- It avoids duplication in the code: the general workflow of the algorithm is implemented once in the abstract class's template method, and necessary variations are implemented in the subclasses
- It controls the point(s) at which specialization is permitted. If the subclasses were to simply override the template method, they could make radical and arbitrary changes to the workflow.

## Example
We have `Operation` class with template method:
```
public abstract class Operation {
    abstract void start();
    abstract void work();
    abstract void finish();

    //Template method
    public final void execute() {
        start();
        work();
        finish();
    }
}
```


And his two childs - `CreateFileOperation` and `DeleteFileOperation` in which we can customize behavior of operation:
```
public final class CreateFileOperation extends Operation {

    private final String fileName;

    public CreateFileOperation(String fileName) {
        this.fileName = fileName;
    }

    @Override
    void start() {
        System.out.println("Start creating file with name = " + fileName);
    }

    @Override
    void work() {
        System.out.println("Creating file with name = " + fileName);
    }

    @Override
    void finish() {
        System.out.println("Finish creating file with name = " + fileName);
    }
}
```

```
public final class DeleteFileOperation extends Operation {

    private final String fileName;

    public DeleteFileOperation(String fileName) {
        this.fileName = fileName;
    }

    @Override
    void start() {
        System.out.println("Start deleting file with name = " + fileName);
    }

    @Override
    void work() {
        System.out.println("Deleting file with name = " + fileName);
    }

    @Override
    void finish() {
        System.out.println("Finish deleting file with name = " + fileName);
    }
}
```

Example of usage:
```
public final class TemplateMethodDemo {

    public void templateMethodExample() {
        String fileName = "SomeFileName";
        Operation create = new CreateFileOperation(fileName);
        Operation delete = new DeleteFileOperation(fileName);
        
        create.execute();
        delete.execute();
    }
}
```

Outputs:
```
Start creating file with name = SomeFileName
Creating file with name = SomeFileName
Finish creating file with name = SomeFileName
Start deleting file with name = SomeFileName
Deleting file with name = SomeFileName
Finish deleting file with name = SomeFileName
```

## Links
https://en.wikipedia.org/wiki/Template_method_pattern
