# Access modifiers

Access modifiers in Java are keywords that determine the scope and visibility of classes, methods, variables, and constructors within an application. They are fundamental to object-oriented programming as they help enforce encapsulation, a core principle restricting direct access to some of an object's components.<sup>[1](https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers#:~:text=Access%20modifiers%20in%20Java%20are,some%20of%20an%20object%27s%20components.)</sup>

There are two levels of access control:<sup>[2](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html#:~:text=Access%20level%20modifiers%20determine%20whether%20other%20classes%20can%20use,protected%2C%20or%20package%2Dprivate%20(no%20explicit%20modifier).)</sup>
- At the top level — `public`, or *package-private* (no explicit modifier);
- At the member level — `public`, `private`, `protected`, or package-private (no explicit modifier).

## Top level

### [Default Access Modifier](https://www.programiz.com/java-programming/access-modifiers#:~:text=are%20visible%20everywhere-,Default%20Access%20Modifier,-If%20we%20do)
If we do not explicitly specify any access modifier for classes then by default the default access modifier is considered.

Example:
```
package defaultPackage;

class Logger {
    void message(){
        System.out.println("This is a message");
    }
}
```

Here, the `Logger` class has the default access modifier. And the class is visible to all the classes that belong to the `defaultPackage` package. However, if we try to use the `Logger` class in another class outside of `defaultPackage`, we will get a compilation error.

### Public Access Modifier
When classes are declared with the `public` modifier then it may be accessed from anywhere. The public access modifier has no scope restriction. 

Example: 
```
package defaultPackage;

public class Logger {
    void message(){
        System.out.println("This is a message");
    }
}
```

Here, the `Logger` class has the `public` access modidier.  It can be accessed from within the class, outside the class, within the package and outside the package.

## Member level

### [`private`](https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers#:~:text=1.-,private,-The%20private%20access)
The `private` access modifier is the most restrictive level of access control. When a member (be it a field, method, or constructor) is declared `private`, it can only be accessed within the same class. No other class, including subclasses in the same package or different packages, can access the member. This modifier is typically used for sensitive data that should be hidden from external classes. 

Example:
```
public class Person {
    private String name;

    private String getName() {
        return this.name;
    }
}
```

In this example, the `name` attribute and the `getName()` method are only accessible within the `Person` class.

### [default (no modifier)](https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers#:~:text=default%20(no%20modifier))
When no access modifier is specified, Java uses a default access level, often called package-private. This means the member is accessible only within classes in the same package. It is less restrictive than `private` but more restrictive than `protected` and `public`. 

Example:
```
class Log {
    void display() {
        System.out.println("Displaying log information");
    }
}
```

In this example, the `Log` class and its method `display()` are accessible only within classes defined in the same package.

### [`protected`](https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers#:~:text=3.-,protected,-The%20protected%20access)
The `protected` access modifier is less restrictive than `private` and default. Members declared as `protected` can be accessed within the same package or in subclasses in different packages. This is particularly useful in cases where you want to hide a member from the world but still make it available to child classes.

Example:
```
public class Animal {
    protected String type;

    protected void displayType() {
        System.out.println("Type: " + type);
    }
}
```

Here, the `type` attribute and the `displayType()` method can be accessed within all classes in the same package and in any subclass of `Animal`, even if those subclasses are in different packages.

### [`public`](https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers#:~:text=4.-,public,-The%20public%20access)
The `public` access modifier is the least restrictive and specifies that the member can be accessed from any other class anywhere, whether within or in a different package. This access level is typically used for methods and variables that must be available consistently to all other classes. 

Example:
```
public class Calculator {
    public int add(int num1, int num2) {
        return num1 + num2;
    }
}
```

In this example, the `add()` method of the `Calculator` class can be accessed by any other class.

## [Access Levels](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html#:~:text=The%20following%20table%20shows%20the%20access,classes%20have%20access%20to%20the%20member.)
The following table shows the access to members permitted by each modifier.
| Modifier  | Class  | Package  | Subclass  | World |
|---|---|---|---|---|
| `public`  |  Y  |  Y  |  Y  |  Y  |
| `protected`  |  Y  |  Y  | Y  |  N  |
| *no modifier*  |  Y  |  Y  |  N  |  N  |
| `private`  |  Y  |  N  |  N  |  N  |

The first data column indicates whether the class itself has access to the member defined by the access level. As you can see, a class always has access to its own members.

The second column indicates whether classes in the same package as the class (regardless of their parentage) have access to the member.

The third column indicates whether subclasses of the class declared outside this package have access to the member.

The fourth column indicates whether all classes have access to the member.

## [Tips on Choosing an Access Level](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html#:~:text=Tips%20on%20Choosing%20an%20Access%20Level%3A%C2%A0)
If other programmers use your class, you want to ensure that errors from misuse cannot happen. Access levels can help you do this.
- Use the most restrictive access level that makes sense for a particular member. Use `private` unless you have a good reason not to;
- Avoid `public` fields except for constants. Public fields tend to link you to a particular implementation and limit your flexibility in changing your code.

# Links
[Access Modifiers in Java: Everything You Need to Know](https://www.simplilearn.com/tutorials/java-tutorial/access-modifiers)

[Controlling Access to Members of a Class](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)

[Java Access Modifiers](https://www.programiz.com/java-programming/access-modifiers)
