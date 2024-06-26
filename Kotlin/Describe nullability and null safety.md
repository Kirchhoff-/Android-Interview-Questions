# Describe nullability and null safety
*Nullability* is the ability of a variable to hold a `null` value. When a variable contains `null`, an attempt to dereference the variable leads to a `NullPointerException`. There are many ways to write code in order to minimize the probability of receiving null pointer exceptions.

## [Support for nullable types](https://kotlinlang.org/docs/java-to-kotlin-nullability-guide.html#support-for-nullable-types)
The most important difference between Kotlin's and Java's type systems is Kotlin's explicit support for nullable types. It is a way to indicate which variables can possibly hold a `null` value. If a variable can be `null`, it's not safe to call a method on the variable because this can cause a `NullPointerException`. Kotlin prohibits such calls at compile time and thereby prevents lots of possible exceptions. At runtime, objects of nullable types and objects of non-nullable types are treated the same: A nullable type isn't a wrapper for a non-nullable type. All checks are performed at compile time. That means there's almost no runtime overhead for working with nullable types in Kotlin.

In Java, if you don't write null checks, methods may throw a `NullPointerException`:
```
// Java
int stringLength(String a) {
    return a.length();
}

void main() {
    stringLength(null); // Throws a `NullPointerException`
}
```

This call will have the following output:
```
java.lang.NullPointerException: Cannot invoke "String.length()" because "a" is null
    at test.java.Nullability.stringLength(Nullability.java:8)
    at test.java.Nullability.main(Nullability.java:12)
    at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
    at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
```

In Kotlin, all regular types are non-nullable by default unless you explicitly mark them as nullable. If you don't expect a to be `null`, declare the `stringLength()` function as follows:
```
// Kotlin
fun stringLength(a: String) = a.length
```

The parameter `a` has the `String` type, which in Kotlin means it must always contain a `String` instance and it cannot contain `null`. Nullable types in Kotlin are marked with a question mark `?`, for example, `String?`. The situation with a `NullPointerException` at runtime is impossible if `a` is `String` because the compiler enforces the rule that all arguments of `stringLength()` not be `null`.

An attempt to pass a `null` value to the `stringLength(a: String)` function will result in a compile-time error, "Null can not be a value of a non-null type String":

![](./res/nullability_compile_time_error.png "Compile time error")

If you want to use this function with any arguments, including `null`, use a question mark after the argument type `String?` and check inside the function body to ensure that the value of the argument is not `null`:
```
// Kotlin
fun stringLength(a: String?): Int = if (a != null) a.length else 0
```

After the check is passed successfully, the compiler treats the variable as if it were of the non-nullable type `String` in the scope where the compiler performs the check.

You can write the same shorter – use the [safe-call operator ?. (If-not-null shorthand)](https://kotlinlang.org/docs/idioms.html#if-not-null-shorthand), which allows you to combine a null check and a method call into a single operation:
```
// Kotlin
fun stringLength(a: String?): Int = a?.length ?: 0
```

## [Default values instead of null](https://kotlinlang.org/docs/java-to-kotlin-nullability-guide.html#default-values-instead-of-null)
Checking for `null` is often used in combination with [setting the default value](https://kotlinlang.org/docs/functions.html#default-arguments) in case the null check is successful.

The Java code with a null check:
```
// Java
Order order = findOrder();
if (order == null) {
    order = new Order(new Customer("Antonio"))
}
```

To express the same in Kotlin, use the [Elvis operator (If-not-null-else shorthand)](https://kotlinlang.org/docs/null-safety.html#elvis-operator):
```
// Kotlin
val order = findOrder() ?: Order(Customer("Antonio"))
```

## [Functions returning a value or null](https://kotlinlang.org/docs/java-to-kotlin-nullability-guide.html#functions-returning-a-value-or-null)
In Java, you need to be careful when working with list elements. You should always check whether an element exists at an index before you attempt to use the element:
```
// Java
var numbers = new ArrayList<Integer>();
numbers.add(1);
numbers.add(2);

System.out.println(numbers.get(0));
//numbers.get(5) // Exception!
```

The Kotlin standard library often provides functions whose names indicate whether they can possibly return a `null` value. This is especially common in the collections API:
```
// Kotlin
// The same code as in Java:
val numbers = listOf(1, 2)

println(numbers[0])  // Can throw IndexOutOfBoundsException if the collection is empty
//numbers.get(5)     // Exception!

// More abilities:
println(numbers.firstOrNull())
println(numbers.getOrNull(5)) // null
```

## [Casting types safely](https://kotlinlang.org/docs/java-to-kotlin-nullability-guide.html#casting-types-safely)
When you need to safely cast a type, in Java you would use the `instanceof` operator and then check how well it worked:
```
// Java
int getStringLength(Object y) {
    return y instanceof String x ? x.length() : -1;
}

void main() {
    System.out.println(getStringLength(1)); // Prints `-1`
}
```

To avoid exceptions in Kotlin, use the [safe cast operator](https://kotlinlang.org/docs/typecasts.html#safe-nullable-cast-operator) `as?`, which returns `null` on failure:
```
// Kotlin
fun main() {
    println(getStringLength(1)) // Prints `-1`
}

fun getStringLength(y: Any): Int {
    val x: String? = y as? String // null
    return x?.length ?: -1 // Returns -1 because `x` is null
}
```

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

# Links
[Nullability in Java and Kotlin](https://kotlinlang.org/docs/java-to-kotlin-nullability-guide.html)

# Further reading
[Null safety](https://kotlinlang.org/docs/null-safety.html)

[Null safety](https://kotlinlang.org/docs/tutorials/kotlin-for-py/null-safety.html)
