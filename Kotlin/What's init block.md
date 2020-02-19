# Init block

A class in Kotlin can have a **primary constructor** and one or more **secondary constructors**. The primary constructor cannot contain any code. Initialization code can be placed in **initializer blocks**, which are prefixed with the `init` keyword.

Code in initializer blocks effectively becomes part of the primary constructor. Delegation to the primary constructor happens as the first statement of a secondary constructor, so the code in all initializer blocks and property initializers is executed before the secondary constructor body. Even if the class has no primary constructor, the delegation still happens implicitly, and the initializer blocks are still executed. During an instance initialization, the initializer blocks are executed in the same order as they appear in the class body. 

## Links
https://kotlinlang.org/docs/reference/classes.html  
https://www.programiz.com/kotlin-programming/constructors
