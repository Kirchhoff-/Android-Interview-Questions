# Lambda expressions
A lambda expression in Kotlin is essentially an anonymous function (a function without a name) that can be treated as a value — meaning it can be assigned to variables, passed as parameters, or returned from other functions.<sup>[1](https://medium.com/@kishoretanwar/what-are-lambda-expressions-in-kotlin-7390c246b0ef#:~:text=A%20lambda%20expression%20in%20Kotlin%20is%20essentially%20an%20anonymous%20function%20(a%20function%20without%20a%20name)%20that%20can%20be%20treated%20as%20a%20value%20%E2%80%94%20meaning%20it%20can%20be%20assigned%20to%20variables%2C%20passed%20as%20parameters%2C%20or%20returned%20from%20other%20functions.)</sup> The following snippet demonstrates the use of two different lambdas in variable assignment expressions:
```
val lambda1 = { println("Hello Lambdas") }
val lambda2 : (String) -> Unit = { name: String -> 
    println("My name is $name") 
}
```

For both of these cases, everything to the right-hand side of the equals sign is the lambda.<sup>[2](https://blog.logrocket.com/a-complete-guide-to-kotlin-lambda-expressions/#:~:text=The%20following%20snippet,is%20the%20lambda.)</sup>

## [Syntax](https://www.scaler.com/topics/kotlin/lambda-expression-and-anonymous-functions-in-kotlin/#:~:text=functional%20programming%20constructs.-,Syntax,a%20%2B%20b%2C%20which%20calculates%20the%20sum%20of%20the%20two%20parameters.,-Passing%20Trailing%20Lambdas)
Lambda expressions in Kotlin are defined using the following syntax:
```
val lambdaName: Type = { argumentList -> codeBody }
```

- `val`. This keyword declares an immutable variable (value) in Kotlin;
- `lambdaName`. This is the name you assign to the variable that will store the lambda function;
- `Type`. This indicates the type of the variable `lambdaName`.  In the context of lambda expressions, the type usually specifies the expected parameter types and the return type of the lambda. For example, `(Int, Int) -> Int` signifies a lambda that takes two `Int` parameters and returns an `Int`;
- `argumentList`. This section lists the parameters that the lambda function accepts. It could be one or more parameters, depending on the function's needs. These parameters are used in the `codeBody` to perform computations or operations;
- `codeBody`. The `codeBody` is the heart of the lambda expression. It contains the code that defines the behavior of the lambda function. It can be a single expression or a block of statements enclosed in curly braces. The result of the last expression (or explicitly returned expression) within the `codeBody` becomes the return value of the lambda function.

Example: 
```
val add: (Int, Int) -> Int = { a, b -> a + b }
```
- **lambdaName** is `add`;
- Type is `(Int, Int) -> Int`, indicating a lambda that takes two `Int` parameters and returns an `Int`;
- **argumentList** is `(a, b)`;
- **codeBody** is `a + b`, which calculates the sum of the two parameters.

### [Invoking a lambda expression](https://blog.logrocket.com/a-complete-guide-to-kotlin-lambda-expressions/#:~:text=Invoking%20a%20lambda%20expression)
Once you’ve defined a lambda expression, how can you invoke the function to actually run the code defined in the lambda body?

As with most things in Kotlin, there are multiple ways for us to invoke a lambda. Take a look at the following examples:
```
val lambda = { greeting: String, name: String -> 
    println("$greeting $name")
}

fun main() {
    lambda("Hello", "Kotlin")
    lambda.invoke("Hello", "Kotlin")
}

// output
Hello Kotlin
Hello Kotlin
```

In this snippet, we’ve defined a lambda that will take two `String`'s and print a greeting. We are able to invoke that lambda in two ways. 

In the first example, we invoke the lambda as if we were calling a named function. We add parentheses to the variable `name`, and pass the appropriate arguments. In the second example, we use a special method available to functional types `invoke()`. 

In both cases, we get the same output. While you may use either option to call your lambda, calling the lambda directly without `invoke()` results in less code and more clearly communicates the semantics of calling a defined function.

## [Passing trailing lambdas﻿](https://kotlinlang.org/docs/lambdas.html#passing-trailing-lambdas)
According to Kotlin convention, if the last parameter of a function is a function, then a lambda expression passed as the corresponding argument can be placed outside the parentheses:
```
val product = items.fold(1) { acc, e -> acc * e }
```

Such syntax is also known as **trailing lambda**.

If the lambda is the only argument in that call, the parentheses can be omitted entirely:
```
run { println("...") }
```

## [`it`: implicit name of a single parameter](https://kotlinlang.org/docs/lambdas.html#it-implicit-name-of-a-single-parameter)
It's very common for a lambda expression to have only one parameter.

If the compiler can parse the signature without any parameters, the parameter does not need to be declared and `->` can be omitted. The parameter will be implicitly declared under the name `it`:
```
ints.filter { it > 0 } // this literal is of type '(it: Int) -> Boolean'
```

## [Returning a value from a lambda expression﻿](https://kotlinlang.org/docs/lambdas.html#returning-a-value-from-a-lambda-expression)
You can explicitly return a value from the lambda using the [qualified return](https://kotlinlang.org/docs/returns.html#return-to-labels) syntax. Otherwise, the value of the last expression is implicitly returned. Therefore, the two following snippets are equivalent:
```
ints.filter {
    val shouldFilter = it > 0
    shouldFilter
}

ints.filter {
    val shouldFilter = it > 0
    return@filter shouldFilter
}
```

This convention, along with [passing a lambda expression outside of parentheses](https://kotlinlang.org/docs/lambdas.html#passing-trailing-lambdas), allows for [LINQ-style](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/) code:
```
strings.filter { it.length == 5 }.sortedBy { it }.map { it.uppercase() }
```

## [Underscore for unused variables﻿](https://kotlinlang.org/docs/lambdas.html#underscore-for-unused-variables)
If the lambda parameter is unused, you can place an underscore instead of its name:
```
map.forEach { (_, value) -> println("$value!") }
```

## [Common Use-Cases of Lambdas in Kotlin](https://medium.com/@kishoretanwar/what-are-lambda-expressions-in-kotlin-7390c246b0ef#:~:text=Common%20Use%2DCases%20of%20Lambdas%20in%20Kotlin)
### [With Higher-Order Functions](https://medium.com/@kishoretanwar/what-are-lambda-expressions-in-kotlin-7390c246b0ef#:~:text=With%20Higher%2DOrder%20Functions)
Functions like `map`, `filter`, `forEach`, and `reduce` often use lambdas:
```
val numbers = listOf(1, 2, 3, 4)
val doubled = numbers.map { it * 2 }
println(doubled) // [2, 4, 6, 8]
```

### [As Function Parameters](https://medium.com/@kishoretanwar/what-are-lambda-expressions-in-kotlin-7390c246b0ef#:~:text=As%20Function%20Parameters)
```
fun operateOnNumbers(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

val result = operateOnNumbers(4, 5) { x, y -> x * y }
println(result)  // Output: 20
```

### [Click Listeners in Android](https://medium.com/@kishoretanwar/what-are-lambda-expressions-in-kotlin-7390c246b0ef#:~:text=Click%20Listeners%20in%20Android)
```
button.setOnClickListener { 
    Toast.makeText(this, "Clicked!", Toast.LENGTH_SHORT).show()
}
```

# Links
[What Are Lambda Expressions in Kotlin?](https://medium.com/@kishoretanwar/what-are-lambda-expressions-in-kotlin-7390c246b0ef)

[A complete guide to Kotlin lambda expressions](https://blog.logrocket.com/a-complete-guide-to-kotlin-lambda-expressions/)

[Lambda Expression and Anonymous Functions in Kotlin](https://www.scaler.com/topics/kotlin/lambda-expression-and-anonymous-functions-in-kotlin/)

[Higher-order functions and lambdas﻿](https://kotlinlang.org/docs/lambdas.html)

# Further reading
[Lambda Expressions in Kotlin](https://www.baeldung.com/kotlin/lambda-expressions)

[Lambdas and Function References](https://typealias.com/start/kotlin-lambdas/)

[Lambda expressions](https://kt.academy/article/fk-lambda-expressions)
