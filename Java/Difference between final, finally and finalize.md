# final, finally, finalize

## Final
The `final` is a modifier can be applied to a variable, a method or a class.

- Apply this modifier to a class-level object, it means that the class can never have subclasses. This is one way to protect your class from being subclassed and often sensitive classes are made final due to security reason.
- Apply this modifier to a method, the method can never be overridden, which means you cannot override the logic of the method in the subclass. This is done to protect the original logic of method.
- Apply this modifier to a variable, the value of the variable remains constant value cannot be changed once assigned.

## Finally

The `finally` block follows `try-catch` block. 

The `finally` block *always* executes when the `try` block exits. This ensures that the `finally` block is executed even if an unexpected exception occurs. But `finally` is useful for more than just exception handling — it allows the programmer to avoid having cleanup code accidentally bypassed by a `return`, `continue`, or `break`. Putting cleanup code in a `finally` block is always a good practice, even when no exceptions are anticipated.

**Note**: If the JVM exits while the `try` or `catch` code is being executed, then the `finally` block may not execute. Likewise, if the thread executing the `try` or `catch` code is interrupted or killed, the `finally` block may not execute even though the application as a whole continues.

## Finalize

The `java.lang.Object.finalize()` called by the garbage collector on an object when garbage collection determines that there are no more references to the object. A subclass overrides the finalize method to dispose of system resources or to perform other cleanup.

The general contract of `finalize()` is that it is invoked if and when the Java virtual machine has determined that there is no longer any means by which this object can be accessed by any thread that has not yet died, except as a result of an action taken by the finalization of some other object or class which is ready to be finalized. The `finalize()` method may take any action, including making this object available again to other threads; the usual purpose of finalize, however, is to perform cleanup actions before the object is irrevocably discarded. For example, the `finalize()` method for an object that represents an input/output connection might perform explicit I/O transactions to break the connection before the object is permanently discarded.

The `finalize()` method of class `Object` performs no special action; it simply returns normally. Subclasses of `Object` may override this definition.

## Links
https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html  
https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#finalize()
