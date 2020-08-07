# Abstract factory pattern

The book **Design Patterns: Elements of Reusable Object-Oriented Software** states that an Abstract Factory "provides an interface for creating families of related or dependent objects without specifying their concrete classes". In other words, this model allows us to create objects that follow a general pattern.

The Abstract Factory design pattern solves problems like:
- How can an application be independent of how its objects are created?
- How can a class be independent of how the objects it requires are created?
- How can families of related or dependent objects be created?

Creating objects directly within the class that requires the objects is inflexible because it commits the class to particular objects and makes it impossible to change the instantiation later independently from (without having to change) the class. It stops the class from being reusable if other objects are required, and it makes the class hard to test because real objects cannot be replaced with mock objects.

The Abstract Factory design pattern describes how to solve such problems:
- Encapsulate object creation in a separate (factory) object. That is, define an interface (AbstractFactory) for creating objects, and implement the interface.
- A class delegates object creation to a factory object instead of creating objects directly.

This makes a class independent of how its objects are created (which concrete classes are instantiated). A class can be configured with a factory object, which it uses to create objects, and even more, the factory object can be exchanged at run-time.

Example:

We have `ManufacturerCountry` enum:

```
public enum ManufacturerCountry {
    USA, JAPAN
}
```

and two interfaces - `Car` and `Motorcycle`

```
public interface Car {
    String info();
}
```

```
public interface Motorcycle {
    String info();
}
```

And different implementations of this interfaces:

```
public class UsaCar implements Car {
    @Override
    public String info() {
        return "USA car info";
    }
}
```

```
public class UsaMotorcycle implements Motorcycle {
    @Override
    public String info() {
        return "USA motorcycle info";
    }
}
```

```
public class JapanCar implements Car {
    @Override
    public String info() {
        return "Japan car info";
    }
}
```

```
public class JapanMotorcycle implements Motorcycle {
    @Override
    public String info() {
        return "Japan motorcycle info";
    }
}
```

Then let's create abstract factory, which is known about all vehicle types:
```
public interface VehicleFactory {
    Motorcycle createMotorcycle();
    Car createCar();
}
```

And two implementations of `VehicleFactory` interface - `UsaVehicleFactory` and `JapanVehicleFactory`:
```
public class UsaVehicleFactory implements VehicleFactory {
    @Override
    public Motorcycle createMotorcycle() {
        return new UsaMotorcycle();
    }

    @Override
    public Car createCar() {
        return new UsaCar();
    }
}
```

```
public class JapanVehicleFactory implements VehicleFactory {
    @Override
    public Motorcycle createMotorcycle() {
        return new JapanMotorcycle();
    }

    @Override
    public Car createCar() {
        return new JapanCar();
    }
}
```

Then create `AbstractVehicleFactory` class which is using a factory and created car and motorcycle.
```
public class AbstractVehicleFactory implements VehicleFactory {

    VehicleFactory factory;

    public AbstractVehicleFactory(ManufacturerCountry country) {
        if (country == ManufacturerCountry.JAPAN) {
            factory = new JapanVehicleFactory();
        } else {
            factory = new UsaVehicleFactory();
        }
    }

    @Override
    public Motorcycle createMotorcycle() {
        return factory.createMotorcycle();
    }

    @Override
    public Car createCar() {
        return factory.createCar();
    }
}
```

and the final demo:
```
public class AbstractFactoryExample {

    void example() {
         AbstractVehicleFactory firstCreator = new AbstractVehicleFactory(ManufacturerCountry.JAPAN);
         AbstractVehicleFactory secondCreator = new AbstractVehicleFactory(ManufacturerCountry.USA);

         Log.d("TAG", "first car info = " + firstCreator.createCar().info());
         Log.d("TAG", "second car info = " + secondCreator.createCar().info());
         Log.d("TAG", "first motorcycle info = " + firstCreator.createMotorcycle().info());
         Log.d("TAG", "second motorcycle info = " + secondCreator.createMotorcycle().info());
    }
}
```

Output:
```
D/TAG: first car info = Japan car info
D/TAG: second car info = USA car info
D/TAG: first motorcycle info = Japan motorcycle info
D/TAG: second motorcycle info = USA motorcycle info
```

Advantages:
- **Isolation of concrete classes**: The Abstract Factory pattern helps you control the classes of objects that an application creates. Because a factory encapsulates the responsibility and the process of creating product objects, it isolates clients from implementation classes. Clients manipulate instances through their abstract interfaces. Product class names are isolated in the implementation of the concrete factory; they do not appear in client code.
- **Exchanging Product Families easily**: The class of a concrete factory appears only once in an application, that is where it’s instantiated. This makes it easy to change the concrete factory an application uses. It can use various product configurations simply by changing the concrete factory. Because an abstract factory creates a complete family of products, the whole product family changes at once.
- **Promoting consistency among products**: When product objects in a family are designed to work together, it’s important that an application use objects from only one family at a time.

Disadvantages:
- **Difficult to support new kind of products**: Extending abstract factories to produce new kinds of Products isn’t easy. That’s because the AbstractFactory interface fixes the set of products that can be created. Supporting new kinds of products requires extending the factory interface, which involves changing the AbstractFactory class and all of its subclasses.

## Links
https://en.wikipedia.org/wiki/Abstract_factory_pattern  
https://www.geeksforgeeks.org/abstract-factory-pattern/  
https://www.baeldung.com/java-abstract-factory-pattern
