# Prototype pattern
The prototype pattern is a creational design pattern. It is used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.

Prototype allows us to hide the complexity of making new instances from the client. The concept is to copy an existing object rather than creating a new instance from scratch, something that may include costly operations. The existing object acts as a prototype and contains the state of the object. The newly copied object may change same properties only if required. This approach saves costly resources and time, especially when the object creation is a heavy process.

The Prototype design pattern solves problems like:
- How can objects be created so that which objects to create can be specified at run-time?
- How can dynamically loaded classes be instantiated?

Creating objects directly within the class that requires (uses) the objects is inflexible because it commits the class to particular objects at compile-time and makes it impossible to specify which objects to create at run-time.

The Prototype design pattern describes how to solve such problems:
- Define a `Prototype` object that returns a copy of itself.
- Create new objects by copying a `Prototype` object.

## Example
We have interface `GameUnit`:

```
public interface GameUnit {
    void setId(String id);
    String getId();
    void setHealth(int value);
    int getHealth();
    void setArmor(int value);
    int getArmor();
    GameUnit copy();
}
```

and implementation of it `AllianceUnit`: 

```
public final class AllianceUnit implements GameUnit, Cloneable {

    private String id;
    private int health;
    private int armor;

    public AllianceUnit() {
        this.id = null;
        this.health = 0;
        this.armor = 0;
    }

    @Override
    public void setId(String id) {
        this.id = id;
    }

    @Override
    public String getId() {
        return id;
    }

    @Override
    public void setHealth(int value) {
        this.health = value;
    }

    @Override
    public int getHealth() {
        return health;
    }

    @Override
    public void setArmor(int value) {
        this.armor = value;
    }

    @Override
    public int getArmor() {
        return armor;
    }

    @Override
    public AllianceUnit copy() {
        AllianceUnit clone;
        try {
            clone = (AllianceUnit) super.clone();
        } catch (CloneNotSupportedException e) {
            //Create new instance
            clone = new AllianceUnit();
        }

        return clone;
    }

    @Override
    public String toString() {
        return "AllianceUnit{" +
                "id='" + id + '\'' +
                ", health=" + health +
                ", armor=" + armor +
                '}';
    }
}
```

Next, let's create `PrototypeStore` class which will be hold prototype for `AllienceUnit` instance and be responsible for creating a new `AllienceUnit` based on this prototype:

```
public final class PrototypeStore {

    private final AllianceUnit allianceUnit;

    public PrototypeStore() {
        allianceUnit = new AllianceUnit();
    }

    public AllianceUnit createAllianceUnit(String id) {
        AllianceUnit prototypedUnit = allianceUnit.copy();

        prototypedUnit.setId(id);
        return prototypedUnit;
    }

    public AllianceUnit createAllianceUnit(String id, int health) {
        AllianceUnit prototypedUnit = allianceUnit.copy();

        prototypedUnit.setId(id);
        prototypedUnit.setHealth(health);
        return prototypedUnit;
    }

    public AllianceUnit createAllianceUnit(String id, int health, int armor) {
        AllianceUnit prototypedUnit = allianceUnit.copy();

        prototypedUnit.setHealth(health);
        prototypedUnit.setId(id);
        prototypedUnit.setArmor(armor);
        return prototypedUnit;
    }
}
```

example of usage `PrototypeStore`:
```
public class PrototypeExample {

    public void example() {
        PrototypeStore store = new PrototypeStore();

        GameUnit first = store.createAllianceUnit("123");
        GameUnit second = store.createAllianceUnit("345", 100);
        GameUnit third = store.createAllianceUnit("789", 150, 50);

        System.out.println("First unit = " + first);
        System.out.println("Second unit = " + second);
        System.out.println("Third unit = " + third);
    }
}
```

## Conclusion

Advantages of Prototype Design Pattern:
- **Adding and removing products at run-time** – Prototypes let you incorporate a new concrete product class into a system simply by registering a prototypical instance with the client. That’s a bit more flexible than other creational patterns, because a client can install and remove prototypes at run-time.
- **Specifying new objects by varying values** - Highly dynamic systems let you define new behavior through object composition by specifying values for an object’s variables and not by defining new classes.
- **Specifying new objects by varying structure** - Many applications build objects from parts and subparts. For convenience, such applications often let you instantiate complex, user-defined structures to use a specific subcircuit again and again.
- **Reduced subclassing** - Factory Method often produces a hierarchy of Creator classes that parallels the product class hierarchy. The Prototype pattern lets you clone a prototype instead of asking a factory method to make a new object. Hence you don’t need a Creator class hierarchy at all.

Disadvantages of Prototype Design Pattern:
- Overkill for a project that uses very few objects and/or does not have an underlying emphasis on the extension of prototype chains.
- It also hides concrete product classes from the client
- Each subclass of Prototype must implement the `clone()` operation which may be difficult, when the classes under consideration already exist. Also implementing `clone()` can be difficult when their internals include objects that don’t support copying or have circular references.

## Links
https://en.wikipedia.org/wiki/Prototype_pattern  
https://www.geeksforgeeks.org/prototype-design-pattern/  
https://www.baeldung.com/java-pattern-prototype  
