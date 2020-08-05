# Destructuring declarations
Destructuring declarations, or destructuring for short, is a technique for unpacking a class instance into separate variables. This means that you can take an object, and create standalone variables from its class variables, with just a single line of code.

The idea is that you can declare new stand-alone variables in a tuple-like manner, where each variable is equal to the class variable at the same position.

For example, in the next block, the author equals the variable at the first position of a Book, and so on.

```
data class Book(val author: String, val title: String, val year: Int)

// ...
val book = Book("Roberto Bolano", "2666", 2004)

val (author, title, year) = book
if (year > 2016) {
    // ...
}
```

Also we can use destructuring declarations for returning multiple values from the function.  For example, a result object and a status of some sort. A compact way of doing this in Kotlin is to declare a data class and return its instance:
```
data class Result(val result: Int, val status: Status)
fun function(...): Result {
    // computations
    
    return Result(result, status)
}

// Now, to use this function:
val (result, status) = function(...)
```

Destructuring declarations provide nice way to traverse a map:
```
for ((key, value) in map) {
   // do something with the key and the value
}
```

If you don't need a variable in the destructuring declaration, you can place an underscore instead of its name:
```
val (_, status) = getResult()
```

You can use the destructuring declarations syntax for lambda parameters. If a lambda has a parameter of the `Pair` type (or `Map.Entry`, or any other type that has the appropriate `componentN` functions), you can introduce several new parameters instead of one by putting them in parentheses:
```
map.mapValues { entry -> "${entry.value}!" }
map.mapValues { (key, value) -> "$value!" }
```

Note the difference between declaring two parameters and declaring a destructuring pair instead of a parameter:
```
{ a -> ... } // one parameter
{ a, b -> ... } // two parameters
{ (a, b) -> ... } // a destructured pair
{ (a, b), c -> ... } // a destructured pair and another parameter
```

If a component of the destructured parameter is unused, you can replace it with the underscore to avoid inventing its name:
```
map.mapValues { (_, value) -> "$value!" }
```
You can specify the type for the whole destructured parameter or for a specific component separately:
```
map.mapValues { (_, value): Map.Entry<Int, String> -> "$value!" }

map.mapValues { (_, value: String) -> "$value!" }
```

## Links
https://kotlinlang.org/docs/reference/multi-declarations.html  
https://www.kotlindevelopment.com/destructuring-declarations/  
https://proandroiddev.com/kotlin-destructuring-declarations-46aad0ee5261
