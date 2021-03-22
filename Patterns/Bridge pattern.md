# Bridge pattern
The bridge pattern is a design pattern used for *"decouple an abstraction from its implementation so that the two can vary independently"*. The *bridge* uses encapsulation, aggregation, and can use inheritance to separate responsibilities into different classes.

When a class varies often, the features of object-oriented programming become very useful because changes to a program's code can be made easily with minimal prior knowledge about the program. The bridge pattern is useful when both the class and what it does vary often. The class itself can be thought of as the *abstraction* and what the class can do as the *implementation*. The bridge pattern can also be thought of as two layers of abstraction.

The Bridge design pattern solves problems like:
- An abstraction and its implementation should be defined and extended independently from each other.
- A compile-time binding between an abstraction and its implementation should be avoided so that an implementation can be selected at run-time.

When using subclassing, different subclasses implement an abstract class in different ways. But an implementation is bound to the abstraction at compile-time and cannot be changed at run-time.

Bridge pattern describe next solution:
- Separate an abstraction (`Abstraction`) from its implementation (`Implementor`) by putting them in separate class hierarchies.
- Implement the `Abstraction` in terms of (by delegating to) an `Implementor` object.

This enables to configure an `Abstraction` with an `Implementor` object at run-time.

## Example
Suppose we have `Color` interface:
```
public interface Color {
    void applyColor();
}
```

And a few implementation on `Color` interface (`RedColor`, `YellowColor`, `GreenColor`):
```
public final class RedColor implements Color {

    @Override
    public void applyColor() {
        System.out.println("red color");
    }
}
```

```
public final class YellowColor implements Color {
    @Override
    public void applyColor() {
        System.out.println("yellow color");
    }
}
```

```
public final class GreenColor implements Color {

    @Override
    public void applyColor() {
        System.out.println("green color");
    }
}
```

Also we have `Shape` class:
```
public abstract class Shape {

    private final Color color;

    Shape(Color color) {
        this.color = color;
    }

    protected Color withColor() {
        return this.color;
    }

    abstract public void draw();
}
```

And a few childs on `Shape` interface (`Triangle`, `Square`):

```
public final class Triangle extends Shape {

    Triangle(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.print("Draws triangle with ");
        super.withColor().applyColor();
    }
}
```

```
public final class Square extends Shape {

    Square(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.print("Draws square with ");
        super.withColor().applyColor();
    }
}
```

With the help of bridge pattern we decouple the interfaces from implementation (`Color` intreface and `Shape` class). `Shape` class takes an instance of `Color` interface implementator and runs its method. Objects are completely decoupled from one another.

Finally, example of usage:
```
public class BridgeExample {

    public void example() {
        Shape redSquare = new Square(new RedColor());
        Shape yellowTriangle = new Triangle(new YellowColor());
        Shape greenSquare = new Square(new GreenColor());

        redSquare.draw();
        yellowTriangle.draw();
        greenSquare.draw();
    }
}
```

Output:
```
Draws square with red color
Draws triangle with yellow color
Draws square with green color
```

## Conclusion
- Bridge pattern decouple an abstraction from its implementation so that the two can vary independently;
- It is used mainly for implementing platform independence feature;
- It adds one more method level redirection to achieve the objective;
- Publish abstraction interface in a separate inheritance hierarchy, and put the implementation in its own inheritance hierarchy;
- Use bridge pattern to run-time binding of the implementation;
- Use bridge pattern to map orthogonal class hierarchies;
- Bridge is designed up-front to let the abstraction and the implementation vary independently;

# Links
[Bridge pattern](https://en.wikipedia.org/wiki/Bridge_pattern)  
[Bridge Design Pattern](https://www.geeksforgeeks.org/bridge-design-pattern/)
