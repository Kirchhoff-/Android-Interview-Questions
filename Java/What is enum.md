# Enum

An `enum` type is a special data type that enables for a variable to be a set of predefined constants. The variable must be equal to one of the values that have been predefined for it. Common examples include compass directions (values of NORTH, SOUTH, EAST, and WEST) and the days of the week. Enums are used when we know all possible values at compile time.

In the Java programming language, you define an enum type by using the enum keyword. For example, you would specify a days-of-the-week enum type as:
```
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY 
}
```
Because they are constants, the names of an enum type's fields are in uppercase letters.

The `enum` constants have an initial value which starts from 0, 1, 2, 3, and so on. But, we can initialize the specific value to the enum constants by defining fields and constructors. As specified earlier, Enum can have fields, constructors, and methods.

```
public final class App {
    enum Card {
        CLUB(5), DIAMOND(10), HEART(15), SPADE(20);

        private int value;

        Card(int value) {
            this.value = value;
        }
    }

    public static void main(String args[]) {
        for (Card card : Card.values()) {
            System.out.println(card + " " + card.value);
        }
    }
}

Output:
CLUB 5
DIAMOND 10
HEART 15
SPADE 20
```

Java compiler internally adds `values()`, `valueOf()` and `ordinal()` methods within the enum at compile time. It internally creates a static and final class for the enum.
- `values()` returns all values present inside enum
- `ordinal()` returns the index of the enum value
- `valueOf()` returns the value of given constant enum

Points to remember for Java Enum:
- Every `enum` constant is always implicitly `public static final`. Since it is `static`, we can access it by using enum Name. Since it is `final`, we can’t create child enums.
- All enums implicitly extend `java.lang.Enum` class. As a class can only extend one parent in Java, so an enum cannot extend anything else.
- `enum` can implement many `interfaces`.
- `enum` can contain constructor and it is executed separately for each enum constant at the time of `enum` class loading.

## Links
https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html  
http://tutorials.jenkov.com/java/enums.html  
https://www.geeksforgeeks.org/enum-in-java/  
https://www.javatpoint.com/enum-in-java  
