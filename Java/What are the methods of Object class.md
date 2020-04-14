# Object class methods

| Comparable  | Comparator  |
|---|---|
| `public final Class getClass()` | Returns the Class class object of this object. The Class class can further be used to get the metadata of this class. |
| `public int hashCode()` | Returns the hashcode number for this object. |
| `public boolean equals(Object obj)` | Compares the given object to this object. |
| `protected Object clone()` | Creates and returns the exact copy (clone) of this object. |
| `public String toString()` | Returns the string representation of this object. |
| `public final void notify()` | Wakes up single thread, waiting on this object's monitor. |
| `public final void notifyAll()` | Wakes up all the threads, waiting on this object's monitor. |
| `public final void wait()` | Causes the current thread to wait, until another thread notifies (invokes `notify()` or `notifyAll()` method). |
| `public final void wait(long timeout)` | Causes the current thread to wait for the specified milliseconds, until another thread notifies (invokes `notify()` or `notifyAll()` method). |
| `public final void wait(long timeout,int nanos)` | Causes the current thread to wait for the specified milliseconds and nanoseconds, until another thread notifies (invokes `notify()` or `notifyAll()` method). |

## getClass()

You cannot override `getClas`s. The `getClass()` method returns a `Class` object, which has methods you can use to get information about the class, such as its name (`getSimpleName()`), its superclass (`getSuperclass()`), and the interfaces it implements (`getInterfaces()`). The `Class` class, in the `java.lang package`, has a large number of methods (more than 50). For example, you can test to see if the class is an annotation (`isAnnotation()`), an interface (`isInterface()`), or an enumeration (`isEnum()`). You can see what the object's fields are (`getFields()`) or what its methods are (`getMethods()`), and so on.

## hashCode()

The value returned by `hashCode()` is the object's hash code, which is the object's memory address in hexadecimal.

By definition, if two objects are equal, their hash code must also be equal. If you override the `equals()` method, you change the way two objects are equated and `Object`'s implementation of `hashCode()` is no longer valid. Therefore, if you override the `equals()` method, you must also override the `hashCode()` method as well.

## equals(Object obj)

The `equals()` method compares two objects for equality and returns true if they are equal. The `equals()` method provided in the `Object` class uses the identity operator (`==`) to determine whether two objects are equal. For primitive data types, this gives the correct result. For objects, however, it does not. The `equals()` method provided by `Object` tests whether the object references are equal—that is, if the objects compared are the exact same object. You should always override the `equals()` method if the identity operator is not appropriate for your class.

## clone()

If a class, or one of its superclasses, implements the `Cloneable` interface, you can use the `clone()` method to create a copy from an existing object. `Object`'s implementation of this method checks to see whether the object on which `clone()` was invoked implements the `Cloneable` interface. If the object does not, the method throws a `CloneNotSupportedException` exception. If the object on which `clone()` was invoked does implement the `Cloneable` interface, `Object`'s implementation of the `clone()` method creates an object of the same class as the original object and initializes the new object's member variables to have the same values as the original object's corresponding member variables.

## toString()

The Object's `toString()` method returns a `String` representation of the object, which is very useful for debugging. The default `toString()` method for class `Object` returns a string consisting of the name of the class of which the object is an instance, the at-sign character - "@", and the unsigned hexadecimal representation of the hash code of the object. You should always consider overriding the `toString()` method in your classes.

## notify()/notifyAll()

This method is used to wake up all the threads waiting in the waiting queue.

## wait()/wait(long timeout)/wait(long timeout,int nanos)

This method is used to put the current thread in the waiting state until any other thread notifies. The time needs to be specified in the function in milliseconds for which you need to resume the thread execution. **Note**: InterruptedException is thrown by this method.

## Links
https://docs.oracle.com/javase/tutorial/java/IandI/objectclass.html  
https://www.geeksforgeeks.org/object-class-in-java/  
https://www.javatpoint.com/object-class  
https://www.educba.com/object-class-in-java/  
