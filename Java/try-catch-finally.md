# Try-Catch-Finally

Java `try`, `catch` and `finally` blocks helps in writing the application code which may throw exceptions in runtime and gives us a chance to either recover from exception by executing alternate application logic or handle the exception gracefully to report back to the user.

## Try block

The first step in constructing an exception handler is to enclose the code that might throw an exception within a `try` block. In general, a `try` block looks like the following:

*try {  
    code  
}  
catch and finally blocks . . .*

The segment in the example labeled code contains one or more legal lines of code that could throw an exception. 

## Catch block

You associate exception handlers with a `try` block by providing one or more `catch` blocks directly after the `try` block. No code can be between the end of the try block and the beginning of the first catch block.

*try {  
  
*} catch (ExceptionType name) {*  
  
*} catch (ExceptionType name) {  
  
}*

Each `catch` block is an exception handler that handles the type of exception indicated by its argument. The argument type, `ExceptionType`, declares the type of exception that the handler can handle and must be the name of a class that inherits from the `Throwable` class. The handler can refer to the exception with name.

The `catch` block contains code that is executed if and when the exception handler is invoked. The runtime system invokes the exception handler when the handler is the first one in the call stack whose `ExceptionType` matches the type of the exception thrown. The system considers it a match if the thrown object can legally be assigned to the exception handler's argument.

If no exeception is thrown by any of the methods called or statements executed inside the try-block, the catch-block is simply ignored. It will not be executed.

## Finally

The `finally` block *always* executes when the `try` block exits. This ensures that the `finally` block is executed even if an unexpected exception occurs. But `finally` is useful for more than just exception handling — it allows the programmer to avoid having cleanup code accidentally bypassed by a `return`, `continue`, or `break`. Putting cleanup code in a `finally` block is always a good practice, even when no exceptions are anticipated.

**Note**: If the JVM exits while the `try` or `catch` code is being executed, then the `finally` block may not execute. Likewise, if the thread executing the `try` or `catch` code is interrupted or killed, the `finally` block may not execute even though the application as a whole continues.

**Important**: The `finally` block is a key tool for preventing resource leaks. When closing a file or otherwise recovering resources, place the code in a `finally` block to ensure that resource is always recovered.

## Links
https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html  
http://tutorials.jenkov.com/java-exception-handling/basic-try-catch-finally.html  
https://howtodoinjava.com/java/exception-handling/try-catch-finally/  
