# Synchronized keyword

The `synchronized` keyword is all about different threads reading and writing to the same variables, objects and resources. `Synchronized` methods enable a simple strategy for preventing thread interference and memory consistency errors: if an object is visible to more than one thread, all reads or writes to that object's variables are done through `synchronized` methods. When you have two threads that are reading and writing to the same 'resource', say a variable named `foo`, you need to ensure that these threads access the variable in an atomic way. Without the `synchronized` keyword, your thread 1 may not see the change thread 2 made to foo, or worse, it may only be half changed. This would not be what you logically expect. Syntax-wise the `synchronized` keyword takes an `Object` as it's parameter (called a lock object), which is then followed by a `{ block of code }`. Adding `synchronized` keyword to a method definition is equal to the entire method body being wrapped in a synchronized code block with the lock object being this (for instance methods) and `ClassInQuestion.getClass()` (for class methods)
 
- When execution encounters this keyword, the current thread tries to "lock/acquire/own" the lock object and execute the associated block of code after the lock has been acquired. 

- Any writes to variables inside the synchronized code block are guaranteed to be visible to every other thread that similarly executes code inside a synchronized code block using the same lock object.

- Only one thread at a time can hold the lock, during which time all other threads trying to acquire the same lock object will wait (pause their execution). The lock will be released when execution exits the synchronized code block.
 
## Links
https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html
https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html  
https://www.baeldung.com/java-synchronized
