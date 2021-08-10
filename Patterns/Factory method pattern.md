# Factory method pattern
**Factory method pattern** is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.

The Factory Method design pattern solves problems like: 
- How can an object be created so that subclasses can redefine which class to instantiate?
- How can a class defer instantiation to subclasses?

The Factory Method design pattern describes how to solve such problems:
- Define a separate operation (*factory method*) for creating an object;
- Create an object by calling a *factory method*.

This enables writing of subclasses to change the way an object is created (to redefine which class to instantiate).

## Example
This Kotlin example is similar to one in the book Design Patterns.

The MazeGame uses Rooms but it puts the responsibility of creating Rooms to its subclasses which create the concrete classes. The regular game mode could use this template method.

Let's create `Room` class:

```
abstract class Room {
  abstract fun connect(room: Room)
}
```

After that create two inheritors of `Room` class:

```
class OrdinaryRoom: Room() {
  override fun connect(room: Room) {}

  override fun toString(): String {
    return "OrdinaryRoom"
  }
}
```

```
class MagicRoom: Room() {
  override fun connect(room: Room) {}

  override fun toString(): String {
    return "MagicRoom"
  }
}
```

Then create `MazeGame` class, that will be base for all games:

```
abstract class MazeGame {
  val rooms: MutableList<Room> = mutableListOf()
  
  init {
    val room1 = makeRoom()
    val room2 = makeRoom()
    room1.connect(room2)
    rooms.add(room1)
    rooms.add(room2)
  }
  
  abstract fun makeRoom(): Room
}
```

Then create two inheritors of `MazeGame` class:
```
class MagicMazeGame: MazeGame() {
  override fun makeRoom(): Room = MagicRoom()
}
```

```
class OrdinaryMazeGame: MazeGame() {
  override fun makeRoom(): Room = OrdinaryRoom()
}
```

And finally example of usage:
```
class FactoryMethodExample {

  fun example() {
    val mazeGame1 = OrdinaryMazeGame()
    val mazeGame2 = MagicMazeGame()

    println("Rooms in game1:")
    mazeGame1.rooms.forEach { println(it) }

    println("Rooms in game2:")
    mazeGame2.rooms.forEach { println(it) }
  }
}
```

Output:
```
Rooms in game1:
OrdinaryRoom
OrdinaryRoom
Rooms in game2:
MagicRoom
MagicRoom
```

## Conclusion

Advantage of Factory Design Pattern:
- Factory Method Pattern allows the sub-classes to choose the type of objects to create;
- The implementation only interacts with parent abstract class or interface. Therefore, it can work with any classes that implement that interface or that extends that abstract class.

When to use Factory Design Pattern:
- When a class doesn’t know which sub-class should create the instance;
- When a class wants that its sub-classes should specify the objects to be created;
- When the parent classes choose the creation of objects to its sub-classes;
- When you want to offer your library’s users the ability to extend its internal components.

# Links
[Factory method pattern](https://en.wikipedia.org/wiki/Factory_method_pattern)

[Overview Of Factory Method Design Pattern](pattern)
