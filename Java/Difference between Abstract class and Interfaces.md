# Abstract class vs Interface

| Abstract class  | Interface  |
|---|---|
| Abstract class can have abstract and non-abstract methods.  | Interface can have only abstract methods. Since Java 8, it can have default and static methods also.  |
| Abstract class can have final, non-final, static and non-static variables.  | Interface has only static and final variables.  |
| Abstract class can provide the implementation of interface.  | Interface can't provide the implementation of abstract class.  |
| An abstract class can extend another Java class and implement multiple Java interfaces.  |  An interface can extend another Java interface only. |
| Abstract class contains Constructors  | Interface does not contain Constructors  |
| Abstract class can contain access modifies for the subs, functions, properties  | Interface cannot have access modifiers by default everything is assumed as public  |

Consider using abstract classes if any of these statements apply to your situation:
- You want to share code among several closely related classes.
- You expect that classes that extend your abstract class have many common methods or fields, or require access modifiers other than public (such as protected and private).
- You want to declare non-static or non-final fields. This enables you to define methods that can access and modify the state of the object to which they belong.

Consider using interfaces if any of these statements apply to your situation:
- You expect that unrelated classes would implement your interface. For example, the interfaces Comparable and Cloneable are implemented by many unrelated classes.
- You want to specify the behavior of a particular data type, but not concerned about who implements its behavior.
- You want to take advantage of multiple inheritance of type.


## Links
https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html  
https://www.geeksforgeeks.org/difference-between-abstract-class-and-interface-in-java/  
https://www.javatpoint.com/difference-between-abstract-class-and-interface    
https://stackoverflow.com/questions/1913098/what-is-the-difference-between-an-interface-and-abstract-class    
