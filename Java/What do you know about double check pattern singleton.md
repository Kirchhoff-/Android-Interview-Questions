# Double check pattern singleton
*Double-checked locking of Singleton* is a way to ensure only one instance of Singleton class is created through an application life cycle. As the name suggests, in double-checked locking, code checks for an existing instance of Singleton class twice with and without locking to double ensure that no more than one instance of singleton gets created. <sup>[1](https://javarevisited.blogspot.com/2014/05/double-checked-locking-on-singleton-in-java.html#axzz89F4A8ss7:~:text=Double%2Dchecked%20locking,singleton%20gets%20created.)</sup>

## [Need of Double-checked Locking of Singleton Class](https://www.geeksforgeeks.org/java-program-to-demonstrate-the-double-check-locking-for-singleton-class/#:~:text=Need%20of%20Double%2Dchecked%20Locking%20of%20Singleton%20Class)
```
private static Singleton instance;
  
public static Singleton getInstance1()
{
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

- The above code will create multiple instances of Singleton class if called by more than one thread in parallel (known as multithreading);
- The primary solution to the current problem will be to make `getInstance()` method `synchronized`;
- Though it’s thread-safe and solves the issue of multiple instances, it isn’t very efficient. You need to bear cost of synchronization every time you call this method, while synchronization is only needed on first class, when Singleton instance is created.

This brings us to *double checked locking pattern*, where only a critical section of code is locked.  

## Implementation
Here is how double checked locking looks like in Java:
```
class Singleton {
    private static Singleton _instance;

    public static Singleton getInstanceDC() {
        if (_instance == null) { // Single Checked
            synchronized (Singleton.class) {
                if (_instance == null) { // Double checked
                    _instance = new Singleton();
                }
            }
        }
        return _instance;
    }
}
```

On the surface, this method looks perfect, as you only need to pay price for synchronized block one time, but it has still broken until you make `_instance` variable `volatile`.

Without a `volatile` modifier, it's possible for another thread in Java to see half initialized state of `_instance` variable, but with `volatile` variable guaranteeing the happens-before relationship, all the write will happen on volatile `_instance` before any read of `_instance` variable. <sup>[2](https://javarevisited.blogspot.com/2014/05/double-checked-locking-on-singleton-in-java.html#axzz89F4A8ss7:~:text=On%20the%20surface,of%20_instance%20variable.)</sup>

Final example:
```
// Java Program to write double checked locking 
// of Singleton class
  
class Singleton {
    private volatile static Singleton instance;
  
    private Singleton() {}
  
    // 1st version: creates multiple instances if two thread
    // access this method simultaneously
    public static Singleton getInstance1()
    {
        if (instance == null) {
            instance = new Singleton();
        }
  
        return instance;
    }
  
    // 2nd version : this is thread-safe and only
    // creates one instance of Singleton on concurrent
    // environment but it is unnecessarily expensive due to
    // cost of synchronization at every call.
    public static synchronized Singleton getInstance2()
    {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
  
    // 3rd version : An implementation of double checked
    // locking of Singleton. Intention is to reduce cost
    // of synchronization and improve performance, by only
    // locking critical section of code, the code which
    // creates instance of Singleton class.
    public static Singleton getInstance3()
    {
        if (instance == null) {
            synchronized (Singleton.class)
            {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

# Links
[Java Program to Demonstrate the Double-Check Locking For Singleton Class](https://www.geeksforgeeks.org/java-program-to-demonstrate-the-double-check-locking-for-singleton-class/)

[Double Checked Locking on Singleton Class in Java - Example](https://javarevisited.blogspot.com/2014/05/double-checked-locking-on-singleton-in-java.html#axzz89F4A8ss7)

# Further Reading
[Double-Checked Locking with Singleton](https://www.baeldung.com/java-singleton-double-checked-locking)

# Next questions
[What is `volatile` keyword?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20is%20volatile%20keyword.md)
