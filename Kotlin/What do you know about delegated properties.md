# [Delegated properties](https://kotlinlang.org/docs/delegated-properties.html)
There are certain common kinds of properties, that, though you can implement them manually every time you need them, it would be helpful to implement them once and add to a library. Examples include:
- *Lazy* properties: the value gets computed only upon first access;
- *Observable* properties: listeners get notified about changes to this property;
- Storing properties in a *map*, instead of a separate field for each property.

To cover these (and other) cases, Kotlin supports *delegated properties*:
```
class Example {
    var p: String by Delegate()
}
```

The syntax is: `val/var <property name>: <Type> by <expression>`. The expression after `by` is a *delegate*, because `get()` (and `set()`) corresponding to the property will be delegated to its `getValue()` and `setValue()` methods. Property delegates don’t have to implement any interface, but they have to provide a `getValue()` function (and `setValue()` --- for `var` s).

For example:

```
import kotlin.reflect.KProperty

class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name}' in $thisRef.")
    }
}
```

When you read from `p` that delegates to an instance of `Delegate`, the `getValue()` function from `Delegate` is called, so that its first parameter is the object you read `p` from and the second parameter holds a description of `p` itself (for example, you can take its name).

```
val e = Example()
println(e.p)
```

This prints:
```
Example@33a17727, thank you for delegating ‘p’ to me!
```

Similarly, when you assign to `p`, the `setValue()` function is called. The first two parameters are the same, and the third holds the value being assigned:
```
e.p = "NEW"
```

This prints:
```
NEW has been assigned to ‘p’ in Example@33a17727.
```

## [Standard delegates](https://kotlinlang.org/docs/delegated-properties.html#standard-delegates)
The Kotlin standard library provides factory methods for several useful kinds of delegates.

### [Lazy properties](https://kotlinlang.org/docs/delegated-properties.html)
`lazy()` is a function that takes a lambda and returns an instance of `Lazy<T>` which can serve as a delegate for implementing a lazy property: the first call to `get()` executes the lambda passed to `lazy()` and remembers the result, subsequent calls to `get()` simply return the remembered result.

```
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}

fun main() {
    println(lazyValue)
    println(lazyValue)
}
```

By default, the evaluation of lazy properties is *synchronized*: the value is computed only in one thread, and all threads will see the same value. If the synchronization of initialization delegate is not required, so that multiple threads can execute it simultaneously, pass `LazyThreadSafetyMode.PUBLICATION` as a parameter to the `lazy()` function.

And if you're sure that the initialization will always happen on the same thread as the one where you use the property, you can use `LazyThreadSafetyMode.NONE`: it doesn't incur any thread-safety guarantees and the related overhead.

### [Observable properties](https://kotlinlang.org/docs/delegated-properties.html#observable-properties)
`Delegates.observable()` takes two arguments: the initial value and a handler for modifications.

The handler is called every time you assign to the property (*after* the assignment has been performed). It has three parameters: a property being assigned to, the old value and the new one:

```
import kotlin.properties.Delegates

class User {
    var name: String by Delegates.observable("<no name>") {
        prop, old, new ->
        println("$old -> $new")
    }
}

fun main() {
    val user = User()
    user.name = "first"
    user.name = "second"
}
```

If you want to intercept assignments and *veto* them, use `vetoable()` instead of `observable()`. The handler passed to the `vetoable` is called *before* the assignment of a new property value.

## [Delegating to another property](https://kotlinlang.org/docs/delegated-properties.html#delegating-to-another-property)
A property can delegate its getter and setter to another property. Such delegation is available for both top-level and class properties (member and extension). The delegate property can be:

- a top-level property
- a member or an extension property of the same class
- a member or an extension property of another class

To delegate a property to another property, use the proper `::` qualifier in the delegate name, for example, `this::delegate` or `MyClass::delegate`

```
var topLevelInt: Int = 0
class ClassWithDelegate(val anotherClassInt: Int)

class MyClass(var memberInt: Int, val anotherClassInstance: ClassWithDelegate) {
    var delegatedToMember: Int by this::memberInt
    var delegatedToTopLevel: Int by ::topLevelInt

    val delegatedToAnotherClass: Int by anotherClassInstance::anotherClassInt
}
var MyClass.extDelegated: Int by ::topLevelInt
```

## [Storing properties in a map](https://kotlinlang.org/docs/delegated-properties.html#storing-properties-in-a-map)
One common use case is storing the values of properties in a map. This comes up often in applications like parsing JSON or doing other “dynamic” things. In this case, you can use the map instance itself as the delegate for a delegated property.

```
class User(val map: Map<String, Any?>) {
    val name: String by map
    val age: Int     by map
}
```

In this example, the constructor takes a map:
```
val user = User(mapOf(
    "name" to "John Doe",
    "age"  to 25
))
```

Delegated properties take values from this map (by the string keys – names of properties):
```
println(user.name) // Prints "John Doe"
println(user.age)  // Prints 25
```

This works also for `var` ’s properties if you use a `MutableMap` instead of read-only `Map`:
```
class MutableUser(val map: MutableMap<String, Any?>) {
    var name: String by map
    var age: Int     by map
}
```

## [Property delegate requirements](https://kotlinlang.org/docs/delegated-properties.html#property-delegate-requirements)
For a *read-only* property (`val`), a delegate should provide an operator function `getValue()` with the following parameters:
- `thisRef` must be the same or a supertype of the *property owner* (for extension properties, it should be the type being extended).
- `property` must be of type `KProperty<*>` or its supertype.

`getValue()` must return the same type as the property (or its subtype).

```
class Resource

class Owner {
    val valResource: Resource by ResourceDelegate()
}

class ResourceDelegate {
    operator fun getValue(thisRef: Owner, property: KProperty<*>): Resource {
        return Resource()
    }
}
```

For a *mutable* property (`var`), a delegate has to additionally provide an operator function `setValue()` with the following parameters:
- `thisRef` must be the same or a supertype of the *property owner* (for extension properties, it should be the type being extended).
- `property` must be of type `KProperty<*>` or its supertype.
- `value` must be of the same type as the property (or its supertype).

```
class Resource

class Owner {
    var varResource: Resource by ResourceDelegate()
}

class ResourceDelegate(private var resource: Resource = Resource()) {
    operator fun getValue(thisRef: Owner, property: KProperty<*>): Resource {
        return resource
    }
    operator fun setValue(thisRef: Owner, property: KProperty<*>, value: Any?) {
        if (value is Resource) {
            resource = value
        }
    }
}
```
`getValue()` and/or `setValue()` functions can be provided either as member functions of the delegate class or extension functions. The latter is handy when you need to delegate property to an object which doesn't originally provide these functions. Both of the functions need to be marked with the `operator` keyword.

# Links
https://kotlinlang.org/docs/delegated-properties.html
