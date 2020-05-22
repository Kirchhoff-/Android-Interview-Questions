# Casting

Casting is the process of making a variable behaves as a variable of another type. If a class shares an IS-A or inheritance relationship with another class or interface, their variables can be cast to each other’s type. Some times the cast is allowed and some times that cast is not allowed. Because, some of the cast will not show any error at compile time, but will fail at run time. 

Here are some basic rules to keep in mind when casting variables:
- Casting an object from a sub class to a super class doesn’t require an explicit cast.
- Casting an object from a super class to a sub class requires an explicit cast.
- The compiler will not allow casts to unrelated types.
- Even when the code compiles without issue, an exception may be thrown at run time if the object being cast is not actually an instance of that class. This will result in the run time exception `ClassCastException`.

## Upcasting
Casting from a subclass to a superclass is called **upcasting**. Typically, the upcasting is implicitly performed by the compiler.
Upcasting is closely related to inheritance – another core concept in Java. It’s common to use reference variables to refer to a more specific type. And every time we do this, implicit upcasting takes place.

```
byte->short->int->long->float->double
```

Automatic Type casting take place when:
- the two types are compatible
- the target type is larger than the source type

Example: 
```
public class Casting
{
    public static void main(String[] args)
    {
      int i = 100;
      long l = i; //no explicit type casting required
      float f = l;  //no explicit type casting required
      System.out.println("Int value "+i);
      System.out.println("Long value "+l);
      System.out.println("Float value "+f);
    }
}
```

## Downcasting
When you are assigning a larger type value to a variable of smaller type, then you need to perform explicit type casting. If we don't perform casting then compiler reports compile time error.

```
double->float->long->int->short->byte
```

Example:
```
public class Test
{
    public static void main(String[] args)
    {
      double d = 100.04;
      long l = (long)d;  //explicit type casting required
      int i = (int)l; //explicit type casting required

      System.out.println("Double value "+d);
      System.out.println("Long value "+l);
      System.out.println("Int value "+i);
    }

}
```

- Downcasting is necessary to gain access to members specific to subclass
- Downcasting is done using cast operator
- To downcast an object safely, we need instanceof operator
- If the real object doesn't match the type we downcast to, then `ClassCastException` will be thrown at runtime

## Links
https://www.baeldung.com/java-type-casting  
https://howtoprogramwithjava.com/java-cast/  
https://www.whizlabs.com/blog/ocajp-casting-java/  
https://www.studytonight.com/java/type-casting-in-java.php  
