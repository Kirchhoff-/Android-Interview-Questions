# Do objects get passed by reference or value?
The two most prevalent modes of passing arguments to methods are “passing-by-value” and “passing-by-reference”. Different programming languages use these concepts in different ways. **As far as Java is concerned, everything is strictly Pass-by-Value**.

The fundamental concepts in any programming language are “values” and “references”. In Java, **Primitive variables store the actual values, whereas Non-Primitives store the reference variables which point to the addresses of the objects they're referring to. Both values and references are stored in the stack memory**.

Arguments in Java are always passed-by-value. During method invocation, a copy of each argument, whether its a value or reference, is created in stack memory which is then passed to the method.

In case of primitives, the value is simply copied inside stack memory which is then passed to the callee method; in case of non-primitives, a reference in stack memory points to the actual data which resides in the heap. When we pass an object, the reference in stack memory is copied and the new reference is passed to the method.

## [Passing Primitive Types](https://www.baeldung.com/java-pass-by-value-or-pass-by-reference#1-passing-primitive-types)

The Java Programming Language features eight primitive data types. **Primitive variables are directly stored in stack memory. Whenever any variable of primitive data type is passed as an argument, the actual parameters are copied to formal arguments and these formal arguments accumulate their own space in stack memory**.

The lifespan of these formal parameters lasts only as long as that method is running, and upon returning, these formal arguments are cleared away from the stack and are discarded.

Example:

```
public class PrimitiveTypesExample {

    public void example() {
        int x1 = 1;
        int x2 = 2;

        System.out.println("Before:");
        System.out.println("X1 = " + x1);
        System.out.println("X2 = " + x2);

        modify(x1, x2);
        System.out.println("After:");
        System.out.println("X1 = " + x1);
        System.out.println("X2 = " + x2);
    }

    private void modify(int x1, int x2) {
        x1 = x1 + 100;
        x2 = x2 + 200;
    }
}
```

Output:
```
Before:
X1 = 1
X2 = 2
After:
X1 = 1
X2 = 2
```

## [Passing Object References](https://www.baeldung.com/java-pass-by-value-or-pass-by-reference#2-passing-object-references)
In Java, all objects are dynamically stored in Heap space under the hood. These objects are referred from references called reference variables.

**Whenever an object is passed as an argument, an exact copy of the reference variable is created which points to the same location of the object in heap memory as the original reference variable.**

**As a result of this, whenever we make any change in the same object in the method, that change is reflected in the original object.** However, if we allocate a new object to the passed reference variable, then it won't be reflected in the original object.

Example:
```
public class NumberHolder {

    private int num;

    NumberHolder(int num) {
        this.num = num;
    }

    public void increment() {
        num = num++;
    }

    public int getValue() {
        return num;
    }
}
```

```
public class NonPrimitiveTypesExample {

    public void example() {
        NumberHolder nh1 = new NumberHolder(1);
        NumberHolder nh2 =  new NumberHolder(2);

        System.out.println("Before:");
        System.out.println("NumberHolder1 = " + nh1.getValue());
        System.out.println("NumberHolder2 = " + nh2.getValue());

        modify(nh1, nh2);
        System.out.println("After:");
        System.out.println("NumberHolder1 = " + nh1.getValue());
        System.out.println("NumberHolder2 = " + nh2.getValue());
    }

    private void modify(NumberHolder nh1, NumberHolder nh2) {
       nh1.increment();

       nh2 = new NumberHolder(100);
       nh2.increment();
    }
}
```

Output:
```
Before:
NumberHolder1 = 1
NumberHolder2 = 2
After:
NumberHolder1 = 2
NumberHolder2 = 2
```

## Passing immutable object references
The JDK contains many immutable classes. Examples include the wrapper types `Integer`, `Double`, `Float`, `Long`, `Boolean`, `BigDecimal`, and of course the very well known `String` class.

In the next example, notice what happens when we change the value of a `String`:

```
public class StringExample {

    public void example() {
        String initialString = "Initial string";

        System.out.println("Before:");
        System.out.println("String = " + initialString);

        modify(initialString);
        System.out.println("After:");
        System.out.println("String = " + initialString);
    }

    private void modify(String str) {
       str = "Modified String";
    }
}
```

Output:
```
Before:
String = Initial string
After:
String = Initial string
```

Nothing changed. That happens because a `String` object is immutable, which means that the fields inside the `String` are final and can’t be changed.

# Links
[Pass-By-Value as a Parameter Passing Mechanism in Java](https://www.baeldung.com/java-pass-by-value-or-pass-by-reference)

[Does Java pass by reference or pass by value?](https://www.infoworld.com/article/3512039/does-java-pass-by-reference-or-pass-by-value.html)


# Further reading
[Is Java “pass-by-reference” or “pass-by-value”?](https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value)

[Are arrays passed by value or passed by reference in Java?](https://stackoverflow.com/questions/12757841/are-arrays-passed-by-value-or-passed-by-reference-in-java)
