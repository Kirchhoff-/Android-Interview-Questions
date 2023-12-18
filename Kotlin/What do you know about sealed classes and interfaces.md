# Sealed classes and interface
**Sealed** classes and interfaces represent restricted class hierarchies that provide more control over inheritance. All direct subclasses of a `sealed class` are known at compile time. No other subclasses may appear outside the module and package within which the `sealed class` is defined. For example, third-party clients can't extend your `sealed class` in their code. Thus, each instance of a `sealed class` has a type from a limited set that is known when this class is compiled.

The same works for sealed interfaces and their implementations: once a module with a `sealed interface` is compiled, no new implementations can appear. <sup>[1](https://kotlinlang.org/docs/sealed-classes.html#:~:text=Sealed%20classes%20and%20interfaces%20represent,no%20new%20implementations%20can%20appear.)</sup>

Before going into details about sealed classes, let's explore what problem they solve. Let's take an example: <sup>[2](https://www.programiz.com/kotlin-programming/sealed-class#:~:text=Before%20going%20into%20details%20about%20sealed%20classes%2C%20let%27s%20explore%20what%20problem%20they%20solve.%20Let%27s%20take%20an%20example)</sup>
```
open class Expr
class Const(val value: Int) : Expr
class Sum(val left: Expr, val right: Expr) : Expr

fun eval(e: Expr): Int = when (e) {
    is Const -> e.value
    is Sum -> eval(e.right) + eval(e.left)
    else -> throw IllegalArgumentException("Unknown expression")
}
```

In the above program, the base class `Expr` has two derived classes `Const` (represents a number) and `Sum` (represents sum of two expressions). Here, it's mandatory to use else branch for default condition in `when` expression.

Now, if you derive a new subclass from `Expr` class, the compiler won't detect anything as `else` branch handles it which can lead to bugs. It would have been better if the compiler issued an error when we added a new subclass.

To solve this problem, you can use `sealed class`. As mentioned, `sealed class` restricts the possibility of creating subclasses. And, when you handle all subclasses of a `sealed class` in an `when` expression, it's not necessary to use `else` branch.

Here's how you can solve the above problem using sealed class:
```
sealed class Expr
class Const(val value: Int) : Expr()
class Sum(val left: Expr, val right: Expr) : Expr()
object NotANumber : Expr()

fun eval(e: Expr): Int = when (e) {
    is Const -> e.value
    is Sum -> eval(e.right) + eval(e.left)
    NotANumber -> java.lang.Double.NaN
}
```

## [Location of direct subclasses](https://kotlinlang.org/docs/sealed-classes.html#location-of-direct-subclasses)
Direct subclasses of sealed classes and interfaces must be declared in the same package. They may be top-level or nested inside any number of other named classes, named interfaces, or named objects. Subclasses can have any visibility as long as they are compatible with normal inheritance rules in Kotlin.

Subclasses of sealed classes must have a proper qualified name. They can't be local nor anonymous objects.

These restrictions don't apply to indirect subclasses. If a direct subclass of a `sealed class` is not marked as sealed, it can be extended in any way that its modifiers allow:
```
sealed interface Error // has implementations only in same package and module

sealed class IOError(): Error // extended only in same package and module
open class CustomError(): Error // can be extended wherever it's visible
```

**Note**. `enum` classes can't extend a `sealed class` (as well as any other class), but they can implement sealed interfaces.

## [Sealed classes and when expression﻿](https://kotlinlang.org/docs/sealed-classes.html#sealed-classes-and-when-expression)
The key benefit of using sealed classes comes into play when you use them in a `when` expression. If it's possible to verify that the statement covers all cases, you don't need to add an `else` clause to the statement:
```
fun log(e: Error) = when(e) {
    is FileReadError -> { println("Error while reading file ${e.file}") }
    is DatabaseError -> { println("Error while reading from database ${e.source}") }
    is RuntimeError ->  { println("Runtime error") }
    // the `else` clause is not required because all the cases are covered
}
```

## [Use Cases of Sealed Classes](https://medium.com/@summitkumar/unlocking-the-power-of-sealed-classes-in-kotlin-design-patterns-and-better-code-organization-5627d73a4903#:~:text=Use%20Cases%20of%20Sealed%20Classes)
Sealed classes are useful in many situations where you have a fixed set of possible classes that need to be represented. Here are some common use cases for sealed classes:

### [Representing the Result of an Operation](https://medium.com/@summitkumar/unlocking-the-power-of-sealed-classes-in-kotlin-design-patterns-and-better-code-organization-5627d73a4903#:~:text=Representing%20the%20Result%20of%20an%20Operation)
One common use case for sealed classes is to represent the result of an operation. For example, We might define a `sealed class` called `Result` with two subclasses: `Success` and `Error`:
```
sealed class Result {
    data class Success(val data: String) : Result()
    data class Error(val message: String) : Result()
}
```

With this definition, We can use a `when` expression to handle all possible cases of `Result`, like this:
```
fun handleResult(result: Result) {
    when(result) {
        is Result.Success -> println(result.data)
        is Result.Error -> println(result.message)
    }
}
```

### [State Machine](https://medium.com/@summitkumar/unlocking-the-power-of-sealed-classes-in-kotlin-design-patterns-and-better-code-organization-5627d73a4903#:~:text=message\)%0A%20%20%20%20%7D%0A%7D-,State%20Machine,-%3A%0AAnother%20common)
Another common use case for sealed classes is to represent the states of a state machine. For example, We might define a `sealed class` called `State` with subclasses representing different states of a game:
```
sealed class State {
    object Initial : State()
    object Running : State()
    object Paused : State()
    object Finished : State()
}
```

With this definition, we can use a `when` expression to handle all possible cases of `State`, like this:
```
fun handleState(state: State) {
    when(state) {
        is State.Initial -> println("The game is starting...")
        is State.Running -> println("The game is running...")
        is State.Paused -> println("The game is paused...")
        is State.Finished -> println("The game is finished!")
    }
}
```

### [Handling UI States](https://medium.com/@summitkumar/unlocking-the-power-of-sealed-classes-in-kotlin-design-patterns-and-better-code-organization-5627d73a4903#:~:text=\)%0A%20%20%20%20%7D%0A%7D-,Handling%20UI%20States,-%3A%0AA%20common)
A common use case for sealed classes is handling different UI states in Android applications. For example, you might define a `sealed class` called `ViewState` subclasses representing different UI states of a screen:
```
sealed class ViewState {
    object Loading : ViewState()
    data class Success(val data: List<String>) : ViewState()
    data class Error(val message: String) : ViewState()
}
```

With this definition, We can use an `when` expression to handle all possible cases of `ViewState`, like this:
```
fun handleViewState(viewState: ViewState) {
    when(viewState) {
      is ViewState.Loading -> showLoadingIndicator()
      is ViewState.Success -> showData(viewState.data)
      is ViewState.Error -> showError(viewState.message)
  }
}
```

## [Sealed classes vs. sealed interfaces](https://blog.logrocket.com/guide-using-sealed-classes-kotlin/#what-sealed-classes:~:text=the%20when%20statement.-,Sealed%20classes%20vs.%20sealed%20interfaces,-As%20explained%20earlier)
As explained earlier, the `sealed` modifier acts on classes and interfaces in the same way. So, you may wonder why you should use sealed interfaces instead of sealed classes. There are at least two good reasons. Let’s dig into them.

### [Kotlin enum classes can implement sealed interfaces](https://blog.logrocket.com/guide-using-sealed-classes-kotlin/#what-sealed-classes:~:text=Kotlin%20enum%20classes%20can%20implement%20sealed%20interfaces)
Sealed interfaces act like standard Koltin interfaces. Considering that enum classes in Kotlin can implement interfaces, this also means that enums can implement sealed interfaces.

In contrast, Kotlin enum classes cannot derive from a class. Therefore, Kotlin enums cannot derive from a `sealed class`. So, this means that the following code is allowed in Kotlin:
```
sealed interface A 

enum class B : A
```

This will compile with no errors.

On the contrary, the snippet below is illegal in Kotlin:
```
sealed class A 

enum class B : A()
```

This will lead to the following error:
```
Enum class cannot inherit from classes
```

### [Sealed interfaces allow multiple inheritance](https://blog.logrocket.com/guide-using-sealed-classes-kotlin/#what-sealed-classes:~:text=Sealed%20interfaces%20allow%20multiple%20inheritance)
Just like what happens for standard interfaces, a Kotlin class can implement more than one sealed interface. This means that sealed interfaces enable the definition of more flexible restricted class hierarchies.

Let’s consider the example below:
```
// to define two new subhierarchies
sealed interface DinnerMenu
sealed interface LunchMenu

sealed class Menu {
    // resticting the Menu hieararchy to LaunchMenu and DinnerMenu
    data class PIZZA(val name: String, val size: String, val quantity: Int): Menu(), DinnerMenu
    data class BURGER(val quantity: Int, val size: String): Menu(), LunchMenu
    data class CHICKEN(val quantity: Int, val pieces: String): Menu(), LunchMenu
}
```

By adding two sealed interfaces, you can restrict the `Menu` hierarchy to two new subhierarchies. In detail, the `LunchMenu` hierarchy will contain both `Menu.BURGER` and `Menu.CHICKEN`. While the `DinnerMenu` hierarchy will contain only `Menu.PIZZA`.

You can then use these new subhierarchies as follows:
```
fun logMenu(menu: Menu) {
    when (menu) {
        is Menu.PIZZA -> print("pizza")
        is Menu.BURGER -> print("burger")
        is Menu.CHICKEN -> print("chicken")
    }
}

fun logLunchMenu(menu: LunchMenu) {
    when (menu) {
        is Menu.BURGER -> print("burger")
        is Menu.CHICKEN -> print("chicken")
    }
}

fun logDinnerMenu(menu: DinnerMenu) {
    when (menu) {
        is Menu.PIZZA -> print("pizza")
    }
}
```

This example shows that the original `Menu` hierarchy still works the same way. What changes is that there are now also the `LunchMenu` and `DinnerMenu` subhierarchies.

# Links 
[Sealed classes and interfaces﻿](https://kotlinlang.org/docs/sealed-classes.html)

[Kotlin Sealed Classes](https://www.programiz.com/kotlin-programming/sealed-class)

[Unlocking the Power of Sealed Classes in Kotlin: Design Patterns and Better Code Organization](https://medium.com/@summitkumar/unlocking-the-power-of-sealed-classes-in-kotlin-design-patterns-and-better-code-organization-5627d73a4903)

[Guide to using sealed classes in Kotlin](https://blog.logrocket.com/guide-using-sealed-classes-kotlin)

# Further reading
[Sealed with a class](https://medium.com/androiddevelopers/sealed-with-a-class-a906f28ab7b5)

[Kotlin — What is a `sealed сlass`?](https://android.jlelse.eu/kotlin-what-is-a-sealed-classe-1e535c416519)

[Abstracting Kotlin Sealed Classes](https://arturdryomov.dev/posts/abstracting-kotlin-sealed-classes/)

[Kotlin Sealed Class (Examples, Uses, How to Use)](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-sealed-class)
