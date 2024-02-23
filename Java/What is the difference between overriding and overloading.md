# Ovverriding vs Overloading

## [Method Overriding](https://levelup.gitconnected.com/method-overriding-and-overloading-in-java-fab89e19b0a7#:~:text=of%20this%20article.%20%F0%9F%9A%80-,Method%20Overriding,-Method%20overriding%2C%20in)
Method overriding, in simple terms, refers to methods that **have the same signature but perform different tasks**.

Let’s assume that we have a class named `Animal` with a method named `eat()`. Now, I have created another class named `Dog`, and I want it to inherit from the `Animal` class. When we perform the inheritance process, we would like to use the `eat()` method inherited from the super class in a way that is suitable for our subclass. Therefore, **we change its functionality**. In other words, we override the `eat()` method.

Example:
```
class Animal{  
  public void eat(){System.out.println("eating...");}  
}  

//Overriding way
class Dog extends Animal{  
  public void eat(){System.out.println("eating bread...");}  
}

class Cat extends Animal{  
  public void eat(){System.out.println("eating fish...");}  
}
```
As we saw in the example, I did not touch the method signatures in any way. I did not change the method name or send a new parameter to that method… **I just played with the internal dynamics of the method and added a completely different functionality**.

Key Rules of Method Overriding<sup>[1](https://www.freecodecamp.org/news/method-overloading-and-overriding-in-java/#:~:text=Key%20Rules%20of%20Method%20Overriding)</sup> :
- The parameter list must not change: the overriding method must take the same number and type of parameters as the overridden method;
- The return type must not change (**Note**: if the method returns an object, a subclass of that object is allowed as the return type);
- The access modifier must be either the same or a less restrictive one (for example, if the overridden method is `protected`, you can declare the overriding method as `public`, but not `private`);
- Thrown checked exceptions, if any, can be removed or reduced by the overriding method. This means that the overriding method can throw the same checked exception as the overridden method, or a subclass of that checked exception, but not a broader exception. This restriction does not apply to unchecked exceptions.

## [Method Overloading](https://levelup.gitconnected.com/method-overriding-and-overloading-in-java-fab89e19b0a7#:~:text=concept%20of%20overloading%20%F0%9F%A4%98%F0%9F%8F%BC.-,Method%20Overloading,-Method%20Overloading%20is)
Method Overloading is **the redefinition of a method with the same name but different parameters**. This means that the parameter section of the method signature is changed. This allows for the creation of different methods that perform the same task but with different parameter types, which can be called independently.

Let’s try to clarify the situation with some short code examples:
```
public class Calculator {
  public int summation(int num1, int num2) {
      return num1 + num2;
  }
  
  public int summation(int num1, int num2, int num3) {
      return num1 + num2 + num3;
  }
  
  public static void main(String[] args) {
      Calculator calc = new Calculator();
      int result1 = calc.summation(2, 3);
      int result2 = calc.summation(2, 3, 4);

      System.out.println("Summation of 2 and 3: " + result1);
      System.out.println("Summation of 2, 3 and 4:  " + result2);
  }
}
```

In the example above, we defined two different methods with the name `summation()` inside the `Calculator` class, where the first method takes **two integer parameters**, and the second method takes **three integer parameters**. In the main method, we call both methods with different parameters and print the result to the screen.

Let’s take a look at another example:
```
public class HelloWorld {
    public void greet() {
        System.out.println("Hello, world!");
    }
    
    public void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }
    
    public static void main(String[] args) {
        HelloWorld helloWorld = new HelloWorld();
        helloWorld.greet();
        helloWorld.greet("John");
    }
}
```

The first method is a simple greeting method that takes **no parameters** and prints the message “Hello, world!”. The second method takes a **parameter of type String** and uses it to create a customized greeting message. Both methods are called in the main method and their results are printed to the console.

Simply put, we can implement method overloading in two different ways<sup>[2](https://www.baeldung.com/java-method-overload-override#:~:text=Simply%20put%2C%20we%20can%20implement%20method%20overloading%20in%20two%20different%20ways%3A)</sup> :
- implementing two or more **methods that have the same name but take different numbers of arguments**;
- implementing two or more **methods that have the same name but take arguments of different types**.

Key Rules of Method Overloading<sup>[3](https://www.freecodecamp.org/news/method-overloading-and-overriding-in-java/#:~:text=Remember%20these%20rules%20when%20overloading%20a%20method%3A)</sup> :
- The overloaded and overloading methods must be in the same class;
- The method parameters must change: either the number or the type of parameters must be different in the two methods;
- The return type can be freely modified;
- The access modifier (`public`, `private`, and so on) can be freely modified;
- Thrown exceptions, if any, can be freely modified.

## [Difference](https://www.scaler.com/topics/overloading-vs-overriding-in-java/#:~:text=A%20list%20of%20more%20differences%20between%20method%20overloading%20and%20method%20overriding%20is%20given%20below%3A)
| Basis  | Method Overloading  | Method Overriding  |
|---|---|---|
|  Definition      |  Method overloading is when two or more methods have the same name and different arguments.	    |  Method overriding is when a subclass modifies a method of superclass having the same signature. |
|  Method Signature	  | The overloaded methods must have different method signatures. It means that the methods must differ in at least one of these: number of parameters, type of parameters, and order of parameters, but should have the same name.	     |   The overridden methods must have the same method signature. It means that method name, number of arguments, order of arguments, and type of arguments should be an exact match.|
|  Return Type	      |  The return type of overloaded methods may or may not have the same return type.	    | If the base class method has a primitive data type, then the overridden method of the subclass must have the same data type.But if the base class method has a derived return type, then the overridden method of the subclass must have either the same data type or subclass of the derived data type.|
|  Access Modifier	  |  We do not have any restriction on access modifiers and may use any access modifier in the overloaded method.	    |  The overridden method in subclass should have the same or less restricted access modifier.|
|  Binding     |  The method call is binded to method definition at compile time. Also known as Static Binding.	    |  The method call is binded to method definition at compile time. Also known as Dynamic Binding.|
|  Polymorphism Type	   | It is an application of compile-time polymorphism.	     |  It is an application of run-time polymorphism.|
|  Scope     | The overloading functions always exist in the same scope.	     |   The overriding function always exists in different scope.|
|  Purpose     | Gives functionality to allow different methods to have the same name and act differently based on the parameters supplied to them.|  Gives functionality to add some extra or unexpected functionality in the subclass as compared to the superclass.|
|  Inheritance Scope	    | Doesn't need inheritance, can be performed within the same class.	     | Relies on inheritance, occurs in classes depicting is-a (parent-child) relationship.|
|  Private Methods	    |  Can be overloaded	    | Cannot be overridden      |
|  Final Methods	      |  Can be overloaded	    |  Cannot be overridden     |
|  Static Methods	      |  Can be overloaded	    |  Cannot be overridden     |
|  Error Handling	      |  If overloading breaks, we get a compile-time error which is relatively easy to fix.	    |  If overriding breaks, we get run-time errors which are more serious and relatively more complex to fix.     |
|  Usage      |  We can overload methods any number of times in a class.	   |  We can override a method only one time in an inheriting class.     |
|  Object Type	      |  Can be performed within a single class or involve static methods.	    | Involves a superclass and its subclass.      |

# Links
[Method Overriding and Overloading in Java](https://levelup.gitconnected.com/method-overriding-and-overloading-in-java-fab89e19b0a7)

[Method Overloading vs Method Overriding in Java – What's the Difference?](https://www.freecodecamp.org/news/method-overloading-and-overriding-in-java/)

[Difference between Method Overloading and Method Overriding in Java](https://www.scaler.com/topics/overloading-vs-overriding-in-java/)

# Further reading
[Difference Between Method Overloading and Overriding](https://testbook.com/key-differences/difference-between-method-overloading-and-overriding) 
