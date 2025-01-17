# `try`-`catch`-`finally`
Java `try`, `catch` and `finally` blocks help in writing the application code which may throw exceptions in runtime and gives us a chance to either recover from exceptions by executing alternate application logic or handle the exception gracefully to report back to the user.<sup>[1](https://howtodoinjava.com/java/exception-handling/try-catch-finally/#:~:text=Java%20try%2C%20catch%20and%20finally%20blocks%20help%20in%20writing%20the%20application%20code%20which%20may%20throw%20exceptions%20in%20runtime%20and%20gives%20us%20a%20chance%20to%20either%20recover%20from%20exceptions%20by%20executing%20alternate%20application%20logic%20or%20handle%20the%20exception%20gracefully%20to%20report%20back%20to%20the%20user.)</sup>. The three blocks are used in the following way:
- The `try` block is used to specify a block of code that may throw an exception;
- The `catch` block is used to handle the exception if it is thrown;
- The `finally` block is used to execute the code after the try and catch blocks have been executed.<sup>[2](https://www.educative.io/answers/how-to-use-the-try-catch-and-finally-blocks-in-java#:~:text=The%20three%20blocks,have%20been%20executed.)</sup>

The following code snippet shows how to use these blocks:<sup>[3](https://www.educative.io/answers/how-to-use-the-try-catch-and-finally-blocks-in-java#:~:text=The%20following%20code%20snippet%20shows%20how%20to%20use%20these%20blocks%3A)</sup>
```
public class Main {
   public static void main(String[] args) {
       try {
          // Code that may throw an exception
       } catch (Exception e) {
          // Code to handle the exception
       } finally {
          // Code that is always executed
       }
   }
}
```

## [`try`](https://docs.oracle.com/javase/tutorial/essential/exceptions/try.html)
The first step in constructing an exception handler is to enclose the code that might throw an exception within a `try` block. In general, a `try` block looks like the following:
```
try {
    code
}
catch and finally blocks . . .
```

The segment in the example labeled `code` contains one or more legal lines of code that could throw an exception. If an exception occurs within the `try` block, that exception is handled by an exception handler associated with it. You should always have an exception handler - either `catch` or `finally` blocks, or both together.

## catch
`catch` block is used to handle the exception by declaring the type of exception within the parameter. The declared exception must be a subclass of the `Throwable` class:
```
try {
    // Code that may throw an exception
} catch (IOException e) {
    // Handle exception
}
```

Rules for `catch` block: 
- More than one `catch` block can be written;
- The `catch` blocks should immediately follow the `try` block;
- The `catch` blocks should all follow each other, without any other statements or blocks in between them.<sup>[4](https://www.examclouds.com/java/ocpjp8/try-catch#:~:text=More%20than%20one,in%20between%20them.)</sup>

An application can go wrong in N different ways. That’s why we can associate multiple `catch` blocks with a single `try` block. In each `catch` block, we can handle one or more specific exceptions in a unique way.

When one `catch` block handles the exception, the next `catch` blocks are not executed. Control shifts directly from the executed `catch` block to execute the remaining part of the program:<sup>[5](https://howtodoinjava.com/java/exception-handling/try-catch-finally/#:~:text=An%20application%20can,the%20finally%20block.)</sup>
```
try {
    //code
}
catch(NullPointerException e) {
    //handle exception
}
catch(NumberFormatException e) {
    //handle exception
}
catch(Exception e) {
    //handle exception
}
```

The good approach is catch the most specific exception first.

## [`finally`](https://jenkov.com/tutorials/java-exception-handling/basic-try-catch-finally.html#:~:text=Virtual%20Machine%20does.-,Finally,-You%20can%20attach)
You can attach a `finally`-clause to a `try`-`catch` block. The code inside the `finally` clause will always be executed, even if an exception is thrown from within the `try` or `catch` block. If your code has a return statement inside the `try` or `catch` block, the code inside the `finally`-block will get executed before returning from the method. Here is how a `finally` clause looks:
```
 public void openFile() {
    FileReader reader = null;
      try {
        reader = new FileReader("someFile");
        int i=0;
        while(i != -1) {
          i = reader.read();
          System.out.println((char) i );
        }
      } catch (IOException e) {
          //do something clever with the exception
      } finally {
        if (reader != null) {
           try {
            reader.close();
           } catch (IOException e) {
            //do something clever with the exception
           }
      }
        System.out.println("--- File End ---");
      }
}
```

No matter whether an exception is thrown or not inside the `try` or `catch` block the code inside the `finally`-block is executed. The example above shows how the file reader is always closed, regardless of the program flow inside the `try` or `catch` block.

**Note**: If an exception is thrown inside a `finally` block, and it is not caught, then that `finally` block is interrupted just like the `try`-block and `catch`-block is. That is why the previous example had the `reader.close()` method call in the `finally` block wrapped in a `try`-`catch` block:
```
} finally {
    if(reader != null) {
        try {
            reader.close();
        } catch (IOException e) {
            //do something clever with the exception
        }
    }
        System.out.println("--- File End ---");
}
```

**Note**: The `finally` block may not execute if the JVM exits while the `try` or `catch` code is being executed.<sup>[6](https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html#:~:text=Note%3A%C2%A0The%20finally%20block%20may%20not%20execute%20if%20the%20JVM%20exits%20while%20the%20try%20or%20catch%20code%20is%20being%20executed.)</sup>

# Links
[Java `try` `catch` `finally`](https://howtodoinjava.com/java/exception-handling/try-catch-finally/)

[How to use the `try`, `catch`, and `finally` blocks in Java](https://www.educative.io/answers/how-to-use-the-try-catch-and-finally-blocks-in-java)

[Try-Catch and Finally Block in Java](https://www.examclouds.com/java/ocpjp8/try-catch)

[Basic `try`-`catch`-`finally` Exception Handling in Java](https://jenkov.com/tutorials/java-exception-handling/basic-try-catch-finally.html)

[Lesson: Exceptions](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)

# Further reading
[Java `try`...`catch`](https://www.programiz.com/java-programming/try-catch)

[Try, Catch and Finally in Java](https://www.scaler.com/topics/java/try-catch-and-finally-in-java/)

[Best practices for using `try`-`catch` blocks to handle exceptions](https://dev.to/binhnt_work/best-practices-for-using-try-catch-blocks-to-handle-exceptions-15pb)
