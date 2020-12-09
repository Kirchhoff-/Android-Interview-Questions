# Strategy pattern
Strategy pattern is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use. Strategy lets the algorithm vary independently from clients that use it. Deferring the decision about which algorithm to use until runtime allows the calling code to be more flexible and reusable.

For instance, a class that performs validation on incoming data may use the strategy pattern to select a validation algorithm depending on the type of data, the source of the data, user choice, or other discriminating factors. These factors are not known until run-time and may require radically different validation to be performed. The validation algorithms (strategies), encapsulated separately from the validating object, may be used by other validating objects in different areas of the system (or even different systems) without code duplication.

## Example

Let's create `Discount` interface:
```
public interface Discount {
    double applyDiscount(double initialPrice);
}
```

and its several implementations (`RegularCustomerDiscount`, `PremiumCustomerDiscount`, `ChristmasDiscount`):
```
public final class RegularCustomerDiscount implements Discount {

    private final static double DISCOUNT_COEFFICIENT = 1;

    @Override
    public double applyDiscount(double initialPrice) {
        return initialPrice * DISCOUNT_COEFFICIENT;
    }
}
```

```
public final class PremiumCustomerDiscount implements Discount {

    private final static double DISCOUNT_COEFFICIENT = 0.95;

    @Override
    public double applyDiscount(double initialPrice) {
        return initialPrice * DISCOUNT_COEFFICIENT;
    }
}
```

```
public final class ChristmasDiscount implements Discount {

    private final static double DISCOUNT_COEFFICIENT = 0.8;

    @Override
    public double applyDiscount(double initialPrice) {
        return initialPrice * DISCOUNT_COEFFICIENT;
    }
}
```

after that, we can apply different discount for customer in runtime, for example:
```
public final class StrategyExample {

    public void example() {
        Customer regularCustomer = new Customer(new RegularCustomerDiscount());
        Customer premiumCustomer = new Customer(new PremiumCustomerDiscount());

        regularCustomer.addToBill(2, 20);
        regularCustomer.addToBill(3, 30);
        regularCustomer.addToBill(4, 40);

        premiumCustomer.addToBill(2, 20);
        premiumCustomer.addToBill(3, 30);
        premiumCustomer.addToBill(4, 40);

        System.out.println("Bill sum for regular customer = " + regularCustomer.getResultBillSum());
        System.out.println("Bill sum for premium customer = " + premiumCustomer.getResultBillSum());

        regularCustomer.applyNewDiscount(new ChristmasDiscount());
        premiumCustomer.applyNewDiscount(new ChristmasDiscount());

        System.out.println("Bill sum for regular customer (with christmas discount) = " + regularCustomer.getResultBillSum());
        System.out.println("Bill sum for premium customer (with christmas discount) = " + premiumCustomer.getResultBillSum());
    }
}
```

Output:
```
Bill sum for regular customer = 290.0
Bill sum for premium customer = 275.5
Bill sum for regular customer (with christmas discount) = 232.0
Bill sum for premium customer (with christmas discount) = 232.0
```

## Conclusion
Advantages:
- A family of algorithms can be defined as a class hierarchy and can be used interchangeably to alter application behavior without changing its architecture.
- By encapsulating the algorithm separately, new algorithms complying with the same interface can be easily introduced.
- The application can switch strategies at run-time.
- Strategy enables the clients to choose the required algorithm, without using a "switch" statement or a series of "if-else" statements.
- Data structures used for implementing the algorithm are completely encapsulated in Strategy classes. Therefore, the implementation of an algorithm can be changed without affecting the Context class.

Disadvantages:
- The application must be aware of all the strategies to select the right one for the right situation.
- Context and the Strategy classes normally communicate through the interface specified by the abstract Strategy base class. Strategy base class must expose interface for all the required behaviours, which some concrete Strategy classes might not implement.
- In most cases, the application configures the Context with the required Strategy object. Therefore, the application needs to create and maintain two objects in place of one.

## Links
https://en.wikipedia.org/wiki/Strategy_pattern  
https://www.geeksforgeeks.org/strategy-pattern-set-1/
