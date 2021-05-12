# Chain of Responsibility
Chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of **processing objects**. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain. A mechanism also exists for adding new processing objects to the end of this chain.

In a variation of the standard chain-of-responsibility model, some handlers may act as dispatchers, capable of sending commands out in a variety of directions, forming a *tree of responsibility*. In some cases, this can occur recursively, with processing objects calling higher-up processing objects with commands that attempt to solve some smaller part of the problem; in this case recursion continues until the command is processed, or the entire tree has been explored.

The Chain of Responsibility design pattern solves problems like:
- Coupling the sender of a request to its receiver should be avoided;
- It should be possible that more than one receiver can handle a request.

The Chain of Responsibility pattern describes how to solve such problems:
- Define a chain of receiver objects having the responsibility, depending on run-time conditions, to either handle a request or forward it to the next receiver on the chain (if any).

This enables us to send a request to a chain of receivers without having to know which one handles the request. The request gets passed along the chain until a receiver handles the request. The sender of a request is no longer coupled to a particular receiver.

Chain of Responsibility pattern is applicable:
- When you want to decouple a request’s sender and receiver;
- Multiple objects, determined at runtime, are candidates to handle a request;
- When you don’t want to specify handlers explicitly in your code;
- When you want to issue a request to one of several objects without specifying the receiver explicitly.

## Example
Suppose we have a certain amount in dollars and we want to dispense it. Let's create `Currency` class which will hold `amount` value:

```
data class Currency(val amount: Int)
```

After that, let's create `DispenseChain` interface which will be the base for all dispensers:

```
interface DispenseChain {
    fun nextChain(nextChain: DispenseChain)
    fun dispense(currency: Currency)
}
```


Next, we will create several implementations of dispensers from which it is possible to create a chain:

```
class DefaultDispenser: DispenseChain {

    override fun nextChain(nextChain: DispenseChain) {
        error("Default dispenser doesn't has next chain")
    }

    override fun dispense(currency: Currency) {
        println("Reminder: ${currency.amount}")
    }
}
```

```
class Dollar10Dispenser(private var nextChain: DispenseChain): DispenseChain {

    override fun nextChain(nextChain: DispenseChain) {
        this.nextChain = nextChain
    }

    override fun dispense(currency: Currency) {
        if (currency.amount >= 10) {
            val num = currency.amount / 10
            val remainder = currency.amount % 10
            println("Dispensing: $num 10 $ note")

            if (remainder != 0) {
                nextChain.dispense(Currency(remainder))
            }
        } else {
            nextChain.dispense(currency)
        }
    }
}
```

```
class Dollar20Dispenser(private var nextChain: DispenseChain): DispenseChain {

    override fun nextChain(nextChain: DispenseChain) {
        this.nextChain = nextChain
    }

    override fun dispense(currency: Currency) {
        if (currency.amount >= 20) {
            val num = currency.amount / 20
            val remainder = currency.amount % 20
            println("Dispensing: $num 20 $ note")

            if (remainder != 0) {
                nextChain.dispense(Currency(remainder))
            }
        } else {
            nextChain.dispense(currency)
        }
    }
}
```

```
class Dollar50Dispenser(private var nextChain: DispenseChain): DispenseChain {

    override fun nextChain(nextChain: DispenseChain) {
        this.nextChain = nextChain
    }

    override fun dispense(currency: Currency) {
        if (currency.amount >= 50) {
            val num = currency.amount / 50
            val remainder = currency.amount % 50
            println("Dispensing: $num 50 $ note")

            if (remainder != 0) {
                nextChain.dispense(Currency(remainder))
            }
        } else {
            nextChain.dispense(currency)
        }
    }
}
```

After that, we can create chains with different dispensers and they will transfer control to each other. Finally, example of usage:
```
class ChainOfResponsibilityExample {

    fun example() {
        val defaultDispenser = DefaultDispenser()
        val dollar10Dispenser = Dollar10Dispenser(defaultDispenser)
        val dollar20Dispenser = Dollar20Dispenser(dollar10Dispenser)
        val dollar50Dispenser = Dollar50Dispenser(dollar20Dispenser)

        println("Dispense 280 dollars:")
        dollar50Dispenser.dispense(Currency(280))
        println("Dispense 100 dollars:")
        dollar50Dispenser.dispense(Currency(100))
        println("Dispense 15 dollars:")
        dollar50Dispenser.dispense(Currency(15))
    }
}
```

Output:
```
Dispense 280 dollars:
Dispensing: 5 50 $ note
Dispensing: 1 20 $ note
Dispensing: 1 10 $ note
Dispense 100 dollars:
Dispensing: 2 50 $ note
Dispense 15 dollars:
Dispensing: 1 10 $ note
Reminder: 5
```

## Conclusion
Advantages:
- To reduce the coupling degree. Decoupling it will request the sender and receiver;
- Simplified object. The object does not need to know the chain structure;
- Enhance flexibility of object assigned duties. By changing the members within the chain or change their order, allow dynamic adding or deleting responsibility;
- Increase the request processing new class of very convenient.

Disadvantages:
- The request must be received not guarantee;
- The performance of the system will be affected, but also in the code debugging is not easy may cause cycle call;
- It may not be easy to observe the characteristics of operation, due to debug.

# Links
[Chain-of-responsibility pattern](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern)

[Chain of Responsibility Design Pattern](https://www.geeksforgeeks.org/chain-responsibility-design-pattern/)

# Further reading
[Chain of Responsibility](https://www.oodesign.com/chain-of-responsibility-pattern.html)
