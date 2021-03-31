# Flyweight pattern
A flyweight is an object that minimizes memory usage by sharing as much data as possible with other similar objects; it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory. Often some parts of the object state can be shared, and it is common practice to hold them in external data structures and pass them to the objects temporarily when they are used.

A classic example usage of the flyweight pattern is the data structures for graphical representation of characters in a word processor. It might be desirable to have, for each character in a document, a glyph object containing its font outline, font metrics, and other formatting data, but this would amount to hundreds or thousands of bytes for each character. Instead, for every character there might be a reference to a flyweight glyph object shared by every instance of the same character in the document; only the position of each character (in the document and/or the page) would need to be stored internally.

The flyweight design pattern solves problems like:
- Large numbers of objects should be supported efficiently;
- Creating large numbers of objects should be avoided.

When representing large text documents, for example, creating an object for each character in the document would result in a huge number of objects that could not be processed efficiently.
What solution does the Flyweight design pattern describe?

Define `Flyweight` objects that:
- Store intrinsic (invariant) state that can be shared and;
- provide an interface through which extrinsic (variant) state can be passed in.

This enables clients to (1) reuse (share) `Flyweight` objects (instead of creating a new object each time) and (2) pass in extrinsic state when they invoke a `Flyweight` operation. This greatly reduces the number of physically created objects.

## Example
Let's create `Pencil` interface:
```
public interface Pencil {
    void setColor(String color);
    void draw(String content);
}
```

Then create `BrushSize` enum:
```
public enum  BrushSize {
    THIN, THICK
}
```

After that, lets create a few implementation of `Pencil` interface - `ThickPencil` and `ThinPencil`
```
public final class ThickPencil implements Pencil {

    private BrushSize brushSize;
    private String color;

    public ThickPencil(String color) {
        this(BrushSize.THICK, color);
    }

    public ThickPencil(BrushSize brushSize, String color) {
        this.brushSize = brushSize;
        this.color = color;
    }

    @Override
    public void setColor(String color) {

    }

    @Override
    public void draw(String content) {
        System.out.println("Drawing " + brushSize + " content in color : " + this.color);
    }
}
```

```
public final class ThinPencil implements Pencil {

    private BrushSize brushSize;
    private String color;

    public ThinPencil(String color) {
        this(BrushSize.THIN, color);
    }

    public ThinPencil(BrushSize brushSize, String color) {
        this.brushSize = brushSize;
        this.color = color;
    }

    @Override
    public void setColor(String color) {
        this.color = color;
    }

    @Override
    public void draw(String content) {
        System.out.println("Drawing " + brushSize + " content in color : " + this.color);
    }
}
```

That create `PencilFactory` class, that will be cached information about pencils, and reduce amount of memory usage (`Flyweight` pattern):
```
import java.util.HashMap;

public final class PencilFactory {

    private static final HashMap<String, Pencil> pencilMap = new HashMap<>();

    public static Pencil getThinPencil(String color) {
        String key = color + BrushSize.THIN;
        Pencil pencil = pencilMap.get(key);

        if (pencil != null) {
            return pencil;
        } else {
            pencil = new ThinPencil(color);
            pencilMap.put(key, pencil);
        }

        return pencil;
    }

    public static Pencil getThickPencil(String color) {
        String key = color + BrushSize.THICK;
        Pencil pencil = pencilMap.get(key);

        if (pencil != null) {
            return pencil;
        } else {
            pencil = new ThickPencil(color);
            pencilMap.put(key, pencil);
        }

        return pencil;
    }
}
```

and finally, example of usage: 
```
public final class FlyweightExample {

    public void example() {
        Pencil greenThinPencil1 = PencilFactory.getThinPencil("GREEN");
        greenThinPencil1.draw("Some words");

        Pencil greenThinPencil2 = PencilFactory.getThinPencil("GREEN");
        greenThinPencil2.draw("Another words");

        Pencil redThickPencil = PencilFactory.getThickPencil("RED");
        redThickPencil.draw("Hello world!");

        if (greenThinPencil1 == greenThinPencil2) {
            System.out.println("Same objects");
        }
    }
}
```

Output:
```
Drawing THIN content in color : GREEN
Drawing THIN content in color : GREEN
Drawing THICK content in color : RED
Same objects
```

# Links
[Flyweight pattern](https://en.wikipedia.org/wiki/Flyweight_pattern)  
[Flyweight Design Pattern](https://howtodoinjava.com/design-patterns/structural/flyweight-design-pattern/)
