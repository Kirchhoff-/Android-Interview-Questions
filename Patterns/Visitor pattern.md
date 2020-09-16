# Visitor pattern
Visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying the structures. It is one way to follow the open/closed principle. 

The visitor pattern solves problems like:
- Defining a new operation for (some) classes of an object structure without changing the classes.

When new operations are needed frequently and the object structure consists of many unrelated classes, it's inflexible to add new subclasses each time a new operation is required.

The visitor pattern describes how to solve such problems:
- Define a separate (visitor) object that implements an operation to be performed on elements of an object structure;
- Clients traverse the object structure and call a dispatching operation accept(visitor) on an element — that "dispatches" (delegates) the request to the "accepted visitor object". The visitor object then performs the operation on the element ("visits the element").

Moving operations into visitor classes is beneficial when:
- many unrelated operations on an object structure are required;
- the classes that make up the object structure are known and not expected to change;
- new operations need to be added frequently;
- an algorithm involves several classes of the object structure, but it is desired to manage it in one single location;
- an algorithm needs to work across several independent class hierarchies.

A drawback to this pattern, however, is that it makes extensions to the class hierarchy more difficult, as new classes typically require a new `visit` method to be added to each visitor.

## Example
First of all created empty `Visitor` interface:
```
public interface Visitor {
}
```

Then created interface - `Visitable` which will have one method for accept `Visitor`:
```
public interface Visitable {
    void accept(Visitor visitor);
}
```

Then created interface - `Goods` which will be inherit from `Visitable`:
```
public interface Goods extends Visitable {
    String country();
    double price();
    double weight();
}
```

Also create a few implementation of `Goods` interface:
```
public final class Apple implements Goods {

    private final String country;
    private final double price;
    private final double weight;

    Apple(String country, double price, double weight) {
        this.country = country;
        this.price = price;
        this.weight = weight;
    }

    @Override
    public String country() {
        return country;
    }

    @Override
    public double price() {
        return price;
    }

    @Override
    public double weight() {
        return weight;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

```
public final class Car implements Goods {

    private final String country;
    private final double price;
    private final double weight;

    Car(String country, double price, double weight) {
        this.country = country;
        this.price = price;
        this.weight = weight;
    }

    @Override
    public String country() {
        return country;
    }

    @Override
    public double price() {
        return price;
    }

    @Override
    public double weight() {
        return weight;
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

Then return to `Visitor` interface and improve it:
```
public interface Visitor {
    void visit(Apple apple);
    void visit(Car car);
}
```

And create a few implementation of `Visitor` interface:
```
public final class CountryVisitor implements Visitor {

    private final String country;
    private int countryGoodsCount;
    
    CountryVisitor(String country) {
        this.country = country;
        this.countryGoodsCount = 0;
    }

    @Override
    public void visit(Apple apple) {
        if (apple.country().equals(country)) {
            countryGoodsCount++;
        }
    }

    @Override
    public void visit(Car car) {
        if (car.country().equals(country)) {
            countryGoodsCount++;
        }
    }
    
    int getCountryGoodsCount() {
        return countryGoodsCount;
    }
}
```

```
public final class SumWeightVisitor implements Visitor {

    private double sumWeight;

    SumWeightVisitor() {
        this.sumWeight = 0;
    }

    @Override
    public void visit(Apple apple) {
        sumWeight += apple.weight();
    }

    @Override
    public void visit(Car car) {
        sumWeight += car.weight();
    }
    
    double getWeightSum() {
        return sumWeight;
    }
}
```

```
public final class CarPriceVisitor implements Visitor {

    private double resultPrice;

    CarPriceVisitor() {
        this.resultPrice = 0;
    }

    @Override
    public void visit(Apple apple) {
        //Do nothing
    }

    @Override
    public void visit(Car car) {
        resultPrice += car.price();
    }

    public double getResultPrice() {
        return resultPrice;
    }
}
```

And finally example of usage:
```
public class VisitorExample {

    public void example() {
        Car usaCar = new Car("USA", 1000, 200);
        Car ukraineCar = new Car("Ukraine", 2000, 100);

        Apple usaApple = new Apple("USA", 1, 2);
        Apple ukraineApple = new Apple("Ukraine", 1, 1);

        List<Visitable> goodsList = new ArrayList<>();
        goodsList.add(usaCar);
        goodsList.add(ukraineCar);
        goodsList.add(usaApple);
        goodsList.add(ukraineApple);

        CountryVisitor countryVisitor = new CountryVisitor("USA");
        SumWeightVisitor weightVisitor = new SumWeightVisitor();
        CarPriceVisitor carPriceVisitor = new CarPriceVisitor();

        for (Visitable item : goodsList) {
            item.accept(countryVisitor);
            item.accept(weightVisitor);
            item.accept(carPriceVisitor);
        }

        System.out.println("USA goods count = " + countryVisitor.getCountryGoodsCount());
        System.out.println("Total goods weight = " + weightVisitor.getWeightSum());
        System.out.println("Result cars price = " + carPriceVisitor.getResultPrice());
    }
}
```

Output:
```
USA goods count = 2
Total goods weight = 303.0
Result cars price = 3000.0
```

Advantages:
- If the logic of operation changes, then we need to make change only in the visitor implementation rather than doing it in all the item classes;
- Adding a new item to the system is easy, it will require change only in visitor interface and implementation and existing item classes will not be affected.

Disadvantages:
- We should know the return type of core methods at the time of designing otherwise we will have to change the interface and all of its implementations;
- If there are too many implementations of visitor interface, it makes it hard to extend.

## Links
https://en.wikipedia.org/wiki/Visitor_pattern  
https://www.geeksforgeeks.org/visitor-design-pattern/
