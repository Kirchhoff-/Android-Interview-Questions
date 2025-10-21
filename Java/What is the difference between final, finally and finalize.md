# `final`, `finally`, `finalize`

## `final`
The `final` keyword is a modifier that can be applied to a variable, method, or class to restrict their usage, making it impossible to change the value, inherit, or override.

- **Final Variables**. When applied to a variable, it means that the variable can only be assigned once. Once initialized, its value cannot be changed. This is particularly useful when you want to create constants;
- **Final Methods**. When applied to a method, it means that the method cannot be overridden by subclasses. This is often used to enforce immutability or security in classes;
- **Final Classes**: When applied to a class, it means that the class cannot be subclassed. This is often used to prevent modification or extension of certain classes, particularly utility classes.<sup>[1](https://www.geekster.in/articles/final-keyword-in-java/#:~:text=Final%20Variables%3A%20When,particularly%20utility%20classes.)</sup>

### [Final Variables](https://www.geekster.in/articles/final-keyword-in-java/#:~:text=Final%20Variables%20as%20Constants)
Declare a `final` variable and initialize it with a constant value. Once you initialized the value, its value cannot be changed:
```
public class FinalExample {
    public static final int MAX_VALUE = 100;
    
    public static void main(String[] args) {
        // Access the final variable
        System.out.println("Maximum value: " + MAX_VALUE);
    }
}
```

### [Final Variables in Instance Fields](https://www.geekster.in/articles/final-keyword-in-java/#:~:text=Final%20Variables%20in%20Instance%20Fields)
Declare instance fields as `final` to ensure they can only be initialized once, either during declaration or in the constructor:
```
public class FinalInstanceFieldExample {
    private final int value;
    
    public FinalInstanceFieldExample(int value) {
        this.value = value;
    }
    
    public void printValue() {
        System.out.println("Value: " + value);
    }
    
    public static void main(String[] args) {
        FinalInstanceFieldExample example = new FinalInstanceFieldExample(10);
        example.printValue();
    }
}
```

### [Final Methods](https://en.wikipedia.org/wiki/Final_(Java)#:~:text=%7B...%7D%20//%20forbidden-,Final%20methods,-%5Bedit%5D)
A final method cannot be overridden or hidden by subclasses.[3] This is used to prevent unexpected behavior from a subclass altering a method that may be crucial to the function or consistency of the class.[4]

Example :
```
public class Base {
    public       void m1() {...}
    public final void m2() {...}

    public static       void m3() {...}
    public static final void m4() {...}
}

public class Derived extends Base {
    public void m1() {...}  // OK, overriding Base#m1()
    public void m2() {...}  // forbidden

    public static void m3() {...}  // OK, hiding Base#m3()
    public static void m4() {...}  // forbidden
}
```

### [Final Classes](https://www.baeldung.com/java-final#final-classes)
Classes marked as `final` can’t be extended. If we look at the code of Java core libraries, we’ll find many final classes there. One example is the `String` class.<sup>[2](https://www.baeldung.com/java-final#final-classes:~:text=Classes%20marked%20as%20final%20can%E2%80%99t%20be%20extended.%20If%20we%20look%20at%20the%20code%20of%20Java%20core%20libraries%2C%20we%E2%80%99ll%20find%20many%20final%20classes%20there.%20One%20example%20is%20the%20String%20class.)</sup>

Any attempt to inherit from a `final` class will cause a compiler error. To demonstrate this, let’s create the `final class Cat`:<sup>[3](https://www.baeldung.com/java-final#final-classes:~:text=Any%20attempt%20to%20inherit%20from%20a%20final%20class%20will%20cause%20a%20compiler%20error.%20To%20demonstrate%20this%2C%20let%E2%80%99s%20create%20the%20final%20class%20Cat%3A)</sup>
```
public final class Cat {

    private int weight;

    // standard getter and setter
}
```

And let’s try to extend it:
```
public class BlackCat extends Cat {
}
```

We’ll see the compiler error:
```
The type BlackCat cannot subclass the final class Cat
```

### [Final Parameters](https://www.baeldung.com/java-final#4-final-parameters)
The `final` keyword is also legal to put before method parameters. A final parameter can’t be changed inside a method:
```
public void methodWithFinalArguments(final int x) {
    x=1;
}
```

The above assignment causes the compiler error:
```
The final local variable x cannot be assigned. It must be blank and not using a compound assignment
```


## `finally`

The `finally` keyword is part of the `try`-`catch`-`finally` statement and is responsible for the cleanup of the code and closing of the resources. 

The `finally` block **always** executes when the `try` block exits. This ensures that the `finally` block is executed even if an unexpected exception occurs. But `finally` is useful for more than just exception handling — it allows the programmer to avoid having cleanup code accidentally bypassed by a `return`, `continue`, or `break`. Putting cleanup code in a `finally` block is always a good practice, even when no exceptions are anticipated.<sup>[4](https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html#:~:text=The%20finally%20block%20always,no%20exceptions%20are%20anticipated.)</sup>

**Note**: The `finally` block may not execute if the JVM exits while the `try` or `catch` code is being executed.

### [Basic Usage](https://www.datacamp.com/doc/java/finally#:~:text=Example%201%3A%20Basic%20Usage)
```
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int data = 50 / 0; // This will throw an ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        } finally {
            System.out.println("Finally block is always executed");
        }
    }
}
```

In this example, an `ArithmeticException` is thrown and caught in the `catch` block. Regardless of the exception, the `finally` block executes and prints a message.

### [Finally without Catch](https://www.datacamp.com/doc/java/finally#:~:text=Example%202%3A%20Finally%20without%20Catch)
```
public class FinallyWithoutCatchExample {
    public static void main(String[] args) {
        try {
            System.out.println("Inside try block");
        } finally {
            System.out.println("Finally block executed");
        }
    }
}
```

Here, no exception is thrown, and there is no `catch` block. The `finally` block executes after the `try` block, printing its message.

### [Resource Cleanup](https://www.datacamp.com/doc/java/finally#:~:text=Example%203%3A-,Resource%20Cleanup,-import%20java.)
```
import java.io.*;

public class FinallyResourceExample {
    public static void main(String[] args) {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("example.txt");
            // Perform file operations
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e);
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    System.out.println("Error closing file: " + e);
                }
            }
        }
    }
}
```

This example demonstrates using the finally block to ensure that a `FileInputStream` is closed, preventing resource leaks.


## [`finalize`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#finalize())
Called by the garbage collector on an object when garbage collection determines that there are no more references to the object. A subclass overrides the `finalize` method to dispose of system resources or to perform other cleanup.

The general contract of `finalize` is that it is invoked if and when the Java virtual machine has determined that there is no longer any means by which this object can be accessed by any thread that has not yet died, except as a result of an action taken by the finalization of some other object or class which is ready to be finalized. The `finalize` method may take any action, including making this object available again to other threads; the usual purpose of `finalize`, however, is to perform cleanup actions before the object is irrevocably discarded. For example, the finalize method for an object that represents an input/output connection might perform explicit I/O transactions to break the connection before the object is permanently discarded.

The `finalize` method of class `Object` performs no special action; it simply returns normally. Subclasses of `Object` may override this definition.

The Java programming language does not guarantee which thread will invoke the `finalize` method for any given object. It is guaranteed, however, that the thread that invokes finalize will not be holding any user-visible synchronization locks when finalize is invoked. If an uncaught exception is thrown by the finalize method, the exception is ignored and finalization of that object terminates.

After the `finalize` method has been invoked for an object, no further action is taken until the Java virtual machine has again determined that there is no longer any means by which this object can be accessed by any thread that has not yet died, including possible actions by other objects or classes which are ready to be finalized, at which point the object may be discarded.

The `finalize` method is never invoked more than once by a Java virtual machine for any given object.

Any exception thrown by the `finalize` method causes the finalization of this object to be halted, but is otherwise ignored.

# Links 
[Final Keyword in Java](https://www.geekster.in/articles/final-keyword-in-java/)

[The “final” Keyword in Java](https://www.baeldung.com/java-final)

[final (Java)](https://en.wikipedia.org/wiki/Final_(Java))

[The finally Block](https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html)

[finally Keyword in Java](https://www.datacamp.com/doc/java/finally)

[Class Object](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html)

# Next questions
[`try`-`catch`-`finally`](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/try-catch-finally.md)

[What is Java's Garbage Collection?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20is%20Java's%20Garbage%20Collection.md)
