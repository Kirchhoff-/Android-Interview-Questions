# `object` keyword
The `object` keyword allow you to define a class and create an instance of it in a single step. This is useful when you need either a reusable singleton instance or a one-time object. Kotlin provides two key approaches for the `object` keyword: 
- **object declarations** for creating singletons;
- **object expressions** for creating anonymous, one-time objects.<sup>[1](https://kotlinlang.org/docs/object-declarations.html#:~:text=objects%20allow%20you,one%2Dtime%20objects.)</sup>

## [Object declarations﻿](https://kotlinlang.org/docs/object-declarations.html#object-declarations-overview)
You can create single instances of objects in Kotlin using object declarations, which always have a name following the `object` keyword. This allows you to define a class and create an instance of it in a single step, which is useful for implementing singletons:
```
// Declares a Singleton object to manage data providers
object DataProviderManager {
    private val providers = mutableListOf<DataProvider>()

    // Registers a new data provider
    fun registerDataProvider(provider: DataProvider) {
        providers.add(provider)
    }

    // Retrieves all registered data providers
    val allDataProviders: Collection<DataProvider> 
        get() = providers
}
```

To refer to the `object`, use its name directly:
```
DataProviderManager.registerDataProvider(exampleProvider)
```

Object declarations can also have supertypes, similar to how anonymous objects can inherit from existing classes or implement interfaces:
```
object DefaultListener : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { ... }

    override fun mouseEntered(e: MouseEvent) { ... }
}
```

Like variable declarations, object declarations are not expressions, so they cannot be used on the right-hand side of an assignment statement:
```
// Syntax error: An object expression cannot bind a name.
val myObject = object MySingleton {
val name = "Singleton"
}
```

Object declarations cannot be local, which means they cannot be nested directly inside a function. However, they can be nested within other object declarations or non-inner classes.

**Note**: The initialization of an object declaration is thread-safe and done on first access.

## [Object expressions﻿](https://kotlinlang.org/docs/object-declarations.html#object-expressions)
Object expressions declare a class and create an instance of that class, but without naming either of them. These classes are useful for one-time use. They can either be created from scratch, inherit from existing classes, or implement interfaces. Instances of these classes are also called **anonymous objects** because they are defined by an expression, not a name.

### [Create anonymous objects from scratch﻿](https://kotlinlang.org/docs/object-declarations.html#create-anonymous-objects-from-scratch)
Object expressions start with the `object` keyword.

If the object doesn't extend any classes or implement interfaces, you can define an object's members directly inside curly braces after the `object` keyword:
```
val helloWorld = object {
    val hello = "Hello"
    val world = "World"
    // Object expressions extend the Any class, which already has a toString() function,
    // so it must be overridden
    override fun toString() = "$hello $world"
}

print(helloWorld)
// Hello World
```

### [Inherit anonymous objects from supertypes﻿](https://kotlinlang.org/docs/object-declarations.html#inherit-anonymous-objects-from-supertypes)
To create an anonymous object that inherits from some type (or types), specify this type after `object` and a colon `:`. Then implement or override the members of this class as if you were inheriting from it:
```
window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { /*...*/ }

    override fun mouseEntered(e: MouseEvent) { /*...*/ }
})
```

If a supertype has a constructor, pass the appropriate constructor parameters to it. Multiple supertypes can be specified, separated by commas, after the colon:
```
// Creates an open class BankAccount with a balance property
open class BankAccount(initialBalance: Int) {
    open val balance: Int = initialBalance
}

// Defines an interface Transaction with an execute() function
interface Transaction {
    fun execute()
}

// A function to perform a special transaction on a BankAccount
fun specialTransaction(account: BankAccount) {
    // Creates an anonymous object that inherits from the BankAccount class and implements the Transaction interface
    // The balance of the provided account is passed to the BankAccount superclass constructor
    val temporaryAccount = object : BankAccount(account.balance), Transaction {

        override val balance = account.balance + 500  // Temporary bonus

        // Implements the execute() function from the Transaction interface
        override fun execute() {
            println("Executing special transaction. New balance is $balance.")
        }
    }
    // Executes the transaction
    temporaryAccount.execute()
}
```

### [Use anonymous objects as return and value types﻿](https://kotlinlang.org/docs/object-declarations.html#use-anonymous-objects-as-return-and-value-types)
When you return an anonymous object from a local or `private` function or property (but not an inline function), all the members of that anonymous object are accessible through that function or property:
```
class UserPreferences {
    private fun getPreferences() = object {
        val theme: String = "Dark"
        val fontSize: Int = 14
    }

    fun printPreferences() {
        val preferences = getPreferences()
        println("Theme: ${preferences.theme}, Font Size: ${preferences.fontSize}")
    }
}
```

This allows you to return an anonymous object with specific properties, offering a simple way to encapsulate data or behavior without creating a separate class.

If a function or property that returns an anonymous object is `public` or `private`, its actual type is:
- `Any` if the anonymous object doesn't have a declared supertype;
- The declared supertype of the anonymous object, if there is exactly one such type;
- The explicitly declared type if there is more than one declared supertype.

In all these cases, members added in the anonymous object are not accessible. Overridden members are accessible if they are declared in the actual type of the function or property.

### [Access variables from anonymous objects﻿](https://kotlinlang.org/docs/object-declarations.html#access-variables-from-anonymous-objects)
Code within the body of object expressions can access variables from the enclosing scope:
```
import java.awt.event.MouseAdapter
import java.awt.event.MouseEvent

fun countClicks(window: JComponent) {
    var clickCount = 0
    var enterCount = 0

    // MouseAdapter provides default implementations for mouse event functions
    // Simulates MouseAdapter handling mouse events
    window.addMouseListener(object : MouseAdapter() {
        override fun mouseClicked(e: MouseEvent) {
            clickCount++
        }

        override fun mouseEntered(e: MouseEvent) {
            enterCount++
        }
    })
    // The clickCount and enterCount variables are accessible within the object expression
}
```

## [Behavior difference between object declarations and expressions﻿](https://kotlinlang.org/docs/object-declarations.html#behavior-difference-between-object-declarations-and-expressions)
There are differences in the initialization behavior between object declarations and object expressions:
- Object expressions are executed (and initialized) **immediately**, where they are used;
- Object declarations are initialized **lazily**, when accessed for the first time;
- A `companion object` is initialized when the corresponding class is loaded (resolved) that matches the semantics of a Java static initializer.

# Links
[Object declarations and expressions﻿](https://kotlinlang.org/docs/object-declarations.html)

# Next questions
[What do you know about `companion object`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20companion%20object.md)

# Further reading
[Understanding the Object Keyword in Kotlin](https://medium.com/softaai-blogs/understanding-the-object-keyword-in-kotlin-5902bc8cd6a4)

[Objects in Kotlin: Create safe singletons in one line of code (KAD 27)](https://antonioleiva.com/objects-kotlin/)

[Object keyword in kotlin](https://medium.com/@appdevinsights/object-keyword-in-kotlin-568cc7f29fc7)
