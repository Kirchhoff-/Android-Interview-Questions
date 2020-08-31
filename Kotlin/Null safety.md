# Null safety
Because nulls are a frequent source of programming errors, Kotlin encourages avoiding them as much as possible - a variable cannot actually be `null` unless it's been declared to allow for `null`, which you do by suffixing the type name with `?`. For example:
```
fun test(a: String, b: String?) {
}
```
The compiler will allow this function to be called as e.g. `test("a", "b")` or `test("a", null)`, but not as `test(null, "b")` or `test(null, null)`. Calling `test(a, b)` is only allowed if the compiler can prove that a cannot possibly be `null`. Inside of test, the compiler will not allow you to do anything with `b` that would result in an exception if `b` should happen to be `null` - so you can do `a.length`, but not `b.length`. However, once you're inside a conditional where you have checked that `b` is not null, you can do it:
```
f (b != null) {
    println(b.length)
}
```
Or:
```
if (b == null) {
    // Can't use members of b in here
} else {
    println(b.length)
}
```
Making frequent null checks is annoying, so if you have to allow for the possibility of nulls, there are several very useful operators in Kotlin to ease working with values that might be `null`.

## Safe call operator
`x?.y` evaluates `x`, and if it is not `null`, it evaluates `x.y` (without reevaluating `x`), whose result becomes the result of the expression - otherwise, you get `null`. This also works for functions, and it can be chained - for example, `x?.y()?.z?.w()` will return null if any of `x`, `x.y()`, or `x.y().z` produce `null`; otherwise, it will return the result of `x.y().z.w()`.

## Elvis operator
`x ?: y` evaluates `x`, which becomes the result of the expression unless it's `null`, in which case you'll get y instead (which ought to be of a non-nullable type). This is also known as the "Elvis operator". You can even use it to perform an early return in case of `null`:
```
val z = x ?: return y
```
This will assign `x` to `z` if `x` is non-null, but if it is `null`, the entire function that contains this expression will stop and return `y` (this works because `return` is also an expression, and if it is evaluated, it evaluates its argument and then makes the containing function return the result).

## Not-null assertion operator
Sometimes, you're in a situation where you have a value `x` that you know is not `null`, but the compiler doesn't realize it. This can legitimately happen when you're interacting with Java code, but if it happens because your code's logic is more complicated than the compiler's ability to reason about it, you should probably restructure your code. If you can't convince the compiler, you can resort to saying `x!!` to form an expression that produces the value of `x`, but whose type is non-nullable:
```
val x: String? = javaFunctionThatYouKnowReturnsNonNull()
val y: String = x!!
```
It can of course be done as a single expression: `val x = javaFunctionThatYouKnowReturnsNonNull()!!`.

`!!` will will raise a `NullPointerException` if the value actually is `null`. So you could also use it if you really need to call a particular function and would rather have an exception if there's no object to call it on (`maybeNull!!.importantFunction()`), although a better solution (because an NPE isn't very informational) is this:
```
val y: String = x ?: throw SpecificException("Useful message")
y.importantFunction()
```
The above could also be a oneliner - and note that the compiler knows that because the `throw` will prevent `y` from coming into existence if `x` is `null`, `y` must be non-null if we reach the line below. Contrast this with `x?.importantFunction()`, which is a no-op if `x` is `null`.

## Using let()
`let()` function executes the lamda function specified only when the reference is non-nullable as shown below.

```
var newString: String? = "Kotlin from Android"
newString?.let { println("The string value is: $it") }
newString = null
newString?.let { println("The string value is: $it") }
```
The statement inside let is a lamda expression. It’s only run when the value of `newString` is not `null`. `it` contains the non-null value of `newString`.

## Using also()
`also()` behaves the same way as `let()` except that it’s generally used to log the values. It can’t assign the value of it to another variable. Here’s an example of `let()` and `also()` changed together.

```
var c = "Hello"

var newString: String? = "Kotlin from Android"
newString?.let { c = it }.also { println("Logging the value: $it") }
```

## Links
https://kotlinlang.org/docs/tutorials/kotlin-for-py/null-safety.html  
https://www.journaldev.com/17568/kotlin-null-safety-nullable
