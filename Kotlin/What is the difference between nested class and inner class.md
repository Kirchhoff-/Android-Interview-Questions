# nested class vs inner class
A nested class is the class that is declared inside another class, for example:
```
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```
Nested classes cannot directly access members (including private members) of the outer class.<sup>[1](https://medium.com/@sandeepkella23/understanding-nested-and-inner-classes-in-kotlin-ae1c4d699053#:~:text=No%20access%20to%20outer%20class%20members%3A%20Nested%20classes%20cannot%20directly%20access%20members%20(including%20private%20members)%20of%20the%20outer%20class)</sup> To create the nested class you don't require an instance of the outher class.

An inner class is a nested class but declared by the adding the keyword `inner`:
```
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```
Inner classes have access to all members (including `private` members) of the outer class. This creates a strong association between the two classes.<sup>[2](https://medium.com/@sandeepkella23/understanding-nested-and-inner-classes-in-kotlin-ae1c4d699053#:~:text=Inner%20classes%20have%20access%20to%20all%20members%20(including%20private%20members)%20of%20the%20outer%20class.%20This%20creates%20a%20strong%20association%20between%20the%20two%20classes.)</sup> To create the inner nested class you require an instance of the outher class.

## [Differences Between Nested and Inner Classes](https://blog.evanemran.info/understanding-inner-classes-and-nested-classes-in-kotlin#:~:text=3.-,Differences%20Between%20Nested%20and%20Inner%20Classes,-Understanding%20the%20differences)
| Feature |  Nested Class  | 	Inner Class  |
|---|---|---|
| Access to Outer Class	 | No |  Yes  |
| Keyword Used	  |  No specific keyword  | `inner` keyword  |
| Use Cases  | Utility classes, logical grouping  | Callbacks, tightly coupled logic  |
| Instantiation  | `OuterClass.NestedClass()`  | `outerInstance.InnerClass()`  |

## [Comparison with Java](https://www.geeksforgeeks.org/kotlin-nested-class-and-inner-class/#:~:text=Comparison%20with%20Java)
Kotlin classes are much similar to Java classes when we think about the capabilities and use cases, but not identical. Nested in Kotlin is similar to a static nested class in Java and the inner class is similar to a non-static nested class in Java.  

| Kotlin | Java |
|---|---|
| Nested class | Static Nested class  |
| Inner class  | Non-static nested class |

## [Best Practices](https://blog.evanemran.info/understanding-inner-classes-and-nested-classes-in-kotlin#:~:text=4.-,Best%20Practices,-Use%20Nested%20Classes)
- **Use Nested Classes for Independence**. If the inner class does not need to interact with the outer class, prefer using a nested class;
- **Use Inner Classes for Encapsulation**. When you need the inner class to work closely with the outer class, encapsulating logic, use an inner class;
- **Keep it Simple**. Avoid overusing nested or inner classes, as they can make the code harder to read if not used judiciously.

# Links
[Nested and inner classes﻿](https://kotlinlang.org/docs/nested-classes.html)

[Understanding Nested and Inner Classes in Kotlin](https://medium.com/@sandeepkella23/understanding-nested-and-inner-classes-in-kotlin-ae1c4d699053)

[Understanding Inner Classes and Nested Classes in Kotlin](https://blog.evanemran.info/understanding-inner-classes-and-nested-classes-in-kotlin)

# Further reading
[What is the purpose of inner class in kotlin?](https://stackoverflow.com/questions/59436139/what-is-the-purpose-of-inner-class-in-kotlin)

[Nested & Inner classes in Kotlin](https://towardsdev.com/nested-inner-classes-in-kotlin-d8620d198602)

[Nested and Inner Class in Kotlin](https://www.scaler.com/topics/kotlin/nested-and-inner-class-in-kotlin/)

[Nested and inner classes](https://www.kotlinprimer.com/classes-what-we-know-from-java/miscellaneous/nested-and-inner-classes/)
