# Decorator pattern
Decorator pattern is a design pattern that allows behavior to be added to an individual object, dynamically, without affecting the behavior of other objects from the same class. The decorator pattern is often useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern.

The Decorator design pattern solves problems like:
- Responsibilities should be added to (and removed from) an object dynamically at run-time.
- A flexible alternative to subclassing for extending functionality should be provided.
- Client-specified embellishment of a core object by recursively wrapping it.

When using subclassing, different subclasses extend a class in different ways. But an extension is bound to the class at compile-time and can't be changed at run-time.

## Example

Suppose we have an interface - `GameUnit`:
```
public interface GameUnit {
    int attackPower();
    int health();
}
```

and base implementation of `GameUnit` interface - `SimpleUnit`: 
```
public final class SimpleUnit implements GameUnit {
    
    private final int attackPower;
    private final int health;
    
    public SimpleUnit() {
        this(1, 1);
    }
    
    public SimpleUnit(int attackPower, int health) {
        this.attackPower = attackPower;
        this.health = health;
    }
    
    @Override
    public int attackPower() {
        return attackPower;
    }

    @Override
    public int health() {
        return health;
    }
}
```

And we need to make two different units, one with weapons and the other with armor. For this we use the decorator pattern:
```
public final class ArmoredUnit implements GameUnit {

    private final GameUnit gameUnit;
    private final int armorValue;

    public ArmoredUnit(GameUnit gameUnit) {
        this(gameUnit, 10);
    }

    public ArmoredUnit(GameUnit gameUnit, int armorValue) {
        this.gameUnit = gameUnit;
        this.armorValue = armorValue;
    }

    @Override
    public int attackPower() {
        return gameUnit.attackPower();
    }

    @Override
    public int health() {
        return gameUnit.health() + armorValue;
    }
}
```

```
public final class ArmedUnit implements GameUnit {

    private final GameUnit gameUnit;
    private final int weaponPower;
    
    public ArmedUnit(GameUnit gameUnit) {
        this(gameUnit, 10);
    }
    
    public ArmedUnit(GameUnit gameUnit, int weaponPower) {
        this.gameUnit = gameUnit;
        this.weaponPower = weaponPower;
    }
    
    @Override
    public int attackPower() {
        return gameUnit.attackPower() + weaponPower;
    }

    @Override
    public int health() {
        return gameUnit.health();
    }
}
```

And now we can decorate `GameUnit`:

```
public final class DecoratorExample {

    public void decoratorExample() {
        GameUnit gameUnit = new SimpleUnit();
        GameUnit armoredUnit = new ArmoredUnit(gameUnit, 10);
        GameUnit armedUnit = new ArmedUnit(gameUnit);

        System.out.println("GameUnit power = " + gameUnit.attackPower() + ", " + "health = " + gameUnit.health());
        System.out.println("ArmoredUnit power = " + armoredUnit.attackPower() + ", " + "health = " + armoredUnit.health());
        System.out.println("ArmedUnit power = " + armedUnit.attackPower() + ", " + "health = " + armedUnit.health());
    }
}
```

Output: 
```
GameUnit power = 1, health = 1
ArmoredUnit power = 1, health = 11
ArmedUnit power = 11, health = 1
```

## Links
https://en.wikipedia.org/wiki/Decorator_pattern  
https://sourcemaking.com/design_patterns/decorator  
https://springframework.guru/gang-of-four-design-patterns/decorator-pattern/
