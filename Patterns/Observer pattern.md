# Observer pattern

The observer pattern is a software design pattern in which an object, named the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

The Observer pattern addresses the following problems:
- A one-to-many dependency between objects should be defined without making the objects tightly coupled;
- It should be ensured that when one object changes state, an open-ended number of dependent objects are updated automatically;
- It should be possible that one object can notify an open-ended number of other objects.

Defining a one-to-many dependency between objects by defining one object (subject) that updates the state of dependent objects directly is inflexible because it couples the subject to particular dependent objects. Still, it can make sense from a performance point of view or if the object implementation is tightly coupled (think of low-level kernel structures that execute thousands of times a second). Tightly coupled objects can be hard to implement in some scenarios, and hard to reuse because they refer to and know about (and how to update) many different objects with different interfaces. In other scenarios, tightly coupled objects can be a better option since the compiler will be able to detect errors at compile-time and optimize the code at the CPU instruction level.

The observer pattern propose next solution:
- Define `Subject` and `Observer` objects.
- When a subject changes state, all registered observers are notified and updated automatically (and probably asynchronously).

The sole responsibility of a subject is to maintain a list of observers and to notify them of state changes by calling their update() operation. The responsibility of observers is to register (and unregister) themselves on a subject (to get notified of state changes) and to update their state (synchronize their state with the subject's state) when they are notified. This makes subject and observers loosely coupled. Subject and observers have no explicit knowledge of each other. Observers can be added and removed independently at run-time. This notification-registration interaction is also known as publish-subscribe.

## Example
Let's create `Observer` interface:
```
public interface Observer {
    void update(Object obj);
}
```

And it's few implementation for different type of data (`IntObserver`, `LongObserver`, `ObjectObserver`):
```
public final class IntObserver implements Observer {

    private final String id;

    IntObserver(String id) {
        this.id = id;
    }

    @Override
    public void update(Object obj) {
        if (obj instanceof Integer) {
            System.out.println("Update IntObserver with id = " + id);
            System.out.println("Update value = " + obj);
        }
    }
}
```

```
public final class LongObserver implements Observer {

    private final String id;

    LongObserver(String id) {
        this.id = id;
    }

    @Override
    public void update(Object obj) {
        if (obj instanceof Long) {
            System.out.println("Update LongObserver with id = " + id);
            System.out.println("Update value = " + obj);
        }
    }
}
```

```
public final class ObjectObserver implements Observer{

    private final String id;

    ObjectObserver(String id) {
        this.id = id;
    }

    @Override
    public void update(Object obj) {
        System.out.println("Update ObjectObserver with id = " + id);
        System.out.println("Update value = " + obj);
    }
}
```

After that, create class - `Subject` which will be hold observers and update them:
```
import java.util.ArrayList;
import java.util.List;

public final class Subject {

    private final List<Observer> observerList;

    Subject() {
        this.observerList = new ArrayList<>();
    }

    void addObserver(Observer observer) {
        observerList.add(observer);
    }

    void removeObserver(Observer observer) {
        observerList.remove(observer);
    }

    void notifyValue(Object obj) {
        for(Observer observer: observerList) {
            observer.update(obj);
        }
    }
}
```

Finally, example of usage `Subject` and `Observer` classes:
```
public final class ObserverExample {

    public void observerExample() {
        Subject subject = new Subject();
        Observer observer1 = new LongObserver("1");
        Observer observer2 = new IntObserver("2");
        Observer observer3 = new ObjectObserver("3");

        subject.addObserver(observer1);
        subject.addObserver(observer2);
        subject.addObserver(observer3);

        subject.notifyValue(new Long(1000));
        subject.notifyValue(new Integer(500));
        subject.notifyValue("Other value");
    }
}
```

Output:
```
Update LongObserver with id = 1
Update value = 1000
Update ObjectObserver with id = 3
Update value = 1000
Update IntObserver with id = 2
Update value = 500
Update ObjectObserver with id = 3
Update value = 500
Update ObjectObserver with id = 3
Update value = Other value
```

## Conclusion
**Advantages**

Provides a loosely coupled design between objects that interact. Loosely coupled objects are flexible with changing requirements. Here loose coupling means that the interacting objects should have less information about each other.

Observer pattern provides this loose coupling as:
- Subject only knows that observer implement Observer interface.Nothing more;
- There is no need to modify Subject to add or remove observers;
- We can reuse subject and observer classes independently of each other.

**Disadvantages**

- Memory leaks caused by [Lapsed listener problem](https://en.wikipedia.org/wiki/Lapsed_listener_problem) because of explicit register and unregistering of observers.

## Links
https://en.wikipedia.org/wiki/Observer_pattern  
https://www.geeksforgeeks.org/observer-pattern-set-1-introduction/  
