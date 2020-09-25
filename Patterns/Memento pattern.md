# Memento pattern
The memento pattern is a software design pattern that provides the ability to restore an object to its previous state (undo via rollback). 
The Memento design pattern defines three distinct roles:
- *Originator* - the object that knows how to save itself.
- *Caretaker* - the object that knows why and when the Originator needs to save and restore itself.
- *Memento* - the lock box that is written and read by the Originator, and shepherded by the Caretaker.

The Memento design pattern solves problems like:
- The internal state of an object should be saved externally so that the object can be restored to this state later.
- The object's encapsulation must not be violated.

The problem is that a well designed object is encapsulated so that its representation (data structure) is hidden inside the object and can't be accessed from outside the object.

The Memento design pattern describes how to solve such problems, make an object (originator) itself responsible for:
- saving its internal state to a (memento) object and
- restoring to a previous state from a (memento) object.

## Example
Create the `Memento` class:
```
public final class Memento {
    
    private String state;
    
    public Memento(String state) {
        this.state = state;
    }
    
    public String getState() { 
        return state;
    }
}
```

than create `Originator` class:

```
public final class Originator {
    private String currentState;

    public void setState(String state) {
        this.currentState = state;
    }

    public String getState() {
        return currentState;
    }

    public Memento saveState() {
        return new Memento(currentState);
    }
    
    public void restoreState(Memento memento) {
        currentState = memento.getState();
    }
}
```

And create the class `Caretaker` which will be hold instances of `Memento` class:
```
import java.util.ArrayList;
import java.util.List;

public final class Caretaker {
    private List<Memento> mementoList = new ArrayList<>();

    public void add(Memento state) {
        mementoList.add(state);
    }

    public Memento get(int index) {
        return mementoList.get(index);
    }
}
```

Example of usage:
```
public final class MementoDemo {

    public void mementoExample() {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();
        
        originator.setState("State 1");
        originator.setState("State 2");
        caretaker.add(originator.saveState());
        
        originator.setState("State 3");
        caretaker.add(originator.saveState());
        
        originator.setState("State 4");
        System.out.println("Current State = " + originator.getState());
        
        originator.restoreState(caretaker.get(0));
        System.out.println("First saved state = " + originator.getState());
        
        originator.restoreState(caretaker.get(1));
        System.out.println("Second saved state = " + originator.getState());
    }
}
```

Output:
```
Current State = State 4
First saved state = State 2
Second saved state = State 3
```

Advantages:
- We can use Serialization to achieve memento pattern implementation that is more generic rather than Memento pattern where every object needs to have it’s own Memento class implementation.

Disadvantage:
- If Originator object is very huge then Memento object size will also be huge and use a lot of memory.

## Links
https://en.wikipedia.org/wiki/Memento_pattern  
https://sourcemaking.com/design_patterns/memento  
https://www.geeksforgeeks.org/memento-design-pattern/  
https://www.journaldev.com/1734/memento-design-pattern-java
