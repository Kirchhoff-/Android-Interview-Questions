# Anonymous classes

Anonymous classes are inner classes with no name. Since they have no name, we can't use them in order to create instances of anonymous classes. As a result, we have to declare and instantiate anonymous classes in a single expression at the point of use. 
Anonymous classes are defined inside an expression, hence it must be a part of a statement, so the semicolon is used at the end of anonymous classes to indicate the end of the expression. 

Anonymous inner class are mainly created in two ways:
- Class (may be abstract or concrete)
- Interface

The anonymous class expression consists of the following:
- The new operator
- The name of an interface to implement or a class to extend.
- Parentheses that contain the arguments to a constructor, just like a normal class instance creation expression. **Note**: When you implement an interface, there is no constructor, so you use an empty pair of parentheses.
- A body, which is a class declaration body. More specifically, in the body, method declarations are allowed but statements are not. 

Restrictions of anonymous classes:
- Cannot have explicitly declared constructors
- Never be abstract
- No way to extend them

Like local classes, anonymous classes can capture variables; they have the same access to local variables of the enclosing scope:
- An anonymous class has access to the members of its enclosing class.
- An anonymous class cannot access local variables in its enclosing scope that are not declared as final or effectively final.
- Like a nested class, a declaration of a type (such as a variable) in an anonymous class shadows any other declarations in the enclosing scope that have the same name.

Anonymous classes also have the same restrictions as local classes with respect to their members:
- Сannot declare static initializers or member interfaces in an anonymous class.
- Сan have static members provided that they are constant variables.

Note that you can declare the following in anonymous classes:
- Fields
- Extra methods (even if they do not implement any methods of the supertype)
- Instance initializers
- Local classes

## Links
https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html  
https://www.geeksforgeeks.org/anonymous-inner-class-java/  
https://www.baeldung.com/java-anonymous-classes  
