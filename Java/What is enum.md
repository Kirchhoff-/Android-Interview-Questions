# Enum

An `enum` type is a special data type that enables for a variable to be a set of predefined constants. The variable must be equal to one of the values that have been predefined for it. Common examples include compass directions (values of NORTH, SOUTH, EAST, and WEST) and the days of the week. You should use `enum` types any time you need to represent a fixed set of constants. 

Because they are constants, the names of an `enum` type's fields are in uppercase letters.

In the Java programming language, you define an enum type by using the `enum` keyword. For example, you would specify a days-of-the-week enum type as<sup>[1](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html#:~:text=An%20enum%20type,set%20of%20constants.)</sup>:
```
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY 
}
```

A Java `enum` is a class type. Although we don’t need to instantiate an `enum` using `new`, it has the same capabilities as other classes. This fact makes Java enumeration a very powerful tool. Just like classes, you can give them constructor, add instance variables and methods, and even implement interfaces.

One thing to keep in mind is that, unlike classes, enumerations neither inherit other classes nor can get extended(i.e become superclass)<sup>[2](https://www.geeksforgeeks.org/enum-in-java/#:~:text=A%20Java%20enumeration,even%20implement%20interfaces.)</sup>

The `enum` class body can include methods and other fields. The compiler automatically adds some special methods when it creates an `enum`. For example, they have a static values method that returns an array containing all of the values of the `enum` in the order they are declared. This method is commonly used in combination with the for-each construct to iterate over the values of an `enum` type. For example, this code from the `Planet` class example below iterates over all the planets in the solar system<sup>[3](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html#:~:text=The%20enum%20class,an%20enum%20type.)</sup>.
```
for (Planet p : Planet.values()) {
    System.out.printf("Your weight on %s is %f%n", p, p.surfaceWeight(mass));
}
```

In the following example, `Planet` is an `enum` type that represents the planets in the solar system. They are defined with constant mass and radius properties.

## [Enum Fields](https://jenkov.com/tutorials/java/enums.html#:~:text=executing%20this%20line.-,Enum%20Fields,of%20the%20enum%20when%20defining%20the%20constants.%20Here%20is%20an%20example%3A,-public%20enum%20Level)
You can add fields to a Java `enum`. Thus, each constant `enum` value gets these fields. The field values must be supplied to the constructor of the `enum` when defining the constants. Here is an example:
```
public enum Level {
    HIGH  (3),  //calls constructor with value 3
    MEDIUM(2),  //calls constructor with value 2
    LOW   (1)   //calls constructor with value 1
    ; // semicolon needed when fields / methods follow


    private final int levelCode;

    private Level(int levelCode) {
        this.levelCode = levelCode;
    }
}
```

Notice how the Java `enum` in the example above has a constructor which takes an `int`. The `enum` constructor sets the `int` field. When the constant `enum` values are defined, an `int` value is passed to the `enum` constructor.

## [Initializing specific values to the enum constants](https://www.javatpoint.com/enum-in-java#:~:text=Initializing%20specific%20values%20to%20the%20enum%20constants)
The `enum` constants have an initial value which starts from 0, 1, 2, 3, and so on. But, we can initialize the specific value to the `enum` constants by defining fields and constructors. As specified earlier, Enum can have fields, constructors, and methods.

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
```

Output:
```
CLUB 5
DIAMOND 10
HEART 15
SPADE 20
```

## [Enum Methods](https://jenkov.com/tutorials/java/enums.html#:~:text=implicitly%20private.-,Enum%20Methods,-You%20can%20add)
You can add methods to a Java `enum` too. Here is an example:
```
public enum Level {
    HIGH  (3),  //calls constructor with value 3
    MEDIUM(2),  //calls constructor with value 2
    LOW   (1)   //calls constructor with value 1
    ; // semicolon needed when fields / methods follow


    private final int levelCode;

    Level(int levelCode) {
        this.levelCode = levelCode;
    }
    
    public int getLevelCode() {
        return this.levelCode;
    }
    
}
```

You call a Java `enum` method via a reference to one of the constant values. Here is Java `enum` method call example:
```
Level level = Level.HIGH;

System.out.println(level.getLevelCode());
```

This code would print out the value `3` which is the value of the `levelCode` field for the enum constant `HIGH`.

## [Enums in if Statements](https://jenkov.com/tutorials/java/enums.html#:~:text=to%20HIGH.-,Enums%20in%20if%20Statements,-Since%20Java%20enums)
Since Java enums are constants you will often have to compare a variable pointing to an enum constant against the possible constants in the enum type. Here is an example of using a Java enum in an `if`-statement:
```
Level level = ...  //assign some Level constant to it

if (level == Level.HIGH) {

} else if (level == Level.MEDIUM) {

} else if (level == Level.LOW) {

}
```

This code compares the `level` variable against each of the possible enum constants in the `Level` enum.

## [Enums in switch Statements](https://jenkov.com/tutorials/java/enums.html#:~:text=executed%20a%20lot.-,Enums%20in%20switch%20Statements,-If%20your%20Java)
If your Java `enum` types contain a lot constants and you need to check a variable against the values as shown in the previous section, using a Java `switch` statement might be a good idea.

You can use enums in switch statements like this:
```
Level level = ...  //assign some Level constant to it

switch (level) {
    case HIGH   : ...; break;
    case MEDIUM : ...; break;
    case LOW    : ...; break;
}
```

Replace the ... with the code to execute if the `level` variable matches the given Level constant value. The code could be a simple Java operation, a method call etc.

## [`Enum` Implementing `Interface`](https://jenkov.com/tutorials/java/enums.html#:~:text=a%20Java%20enum.-,Enum%20Implementing%20Interface,-A%20Java%20Enum)
A Java `Enum` can implement a Java `Interface` in case you feel that makes sense in your situation. Here is an example of a Java `Enum` implementing an interface:
```
public enum EnumImplementingInterface implements MyInterface {
    FIRST("First Value"), SECOND("Second Value");


    private String description = null;

    private EnumImplementingInterface(String desc){
        this.description = desc;
    }

    @Override
    public String getDescription() {
        return this.description;
    }
}
```
It is the method `getDescription()` that comes from the interface `MyInterface`.

# Links
[Enum Types](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)

[Java Enums](http://tutorials.jenkov.com/java/enums.html)

[Java Enums](https://www.javatpoint.com/enum-in-java)

[enum in Java](https://www.geeksforgeeks.org/enum-in-java/)
