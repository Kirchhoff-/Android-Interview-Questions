# Type aliases

## [Main](https://kotlinlang.org/docs/type-aliases.html)
Type aliases provide alternative names for existing types. If the type name is too long you can introduce a different shorter name and use the new one instead.

It's useful to shorten long generic types. For instance, it's often tempting to shrink collection types:
```
typealias NodeSet = Set<Network.Node>

typealias FileTable<K> = MutableMap<K, MutableList<File>>
```

You can provide different aliases for function types:
```
typealias MyHandler = (Int, String, Any) -> Unit

typealias Predicate<T> = (T) -> Boolean
```

You can have new names for inner and nested classes:
```
class A {
    inner class Inner
}
class B {
    inner class Inner
}

typealias AInner = A.Inner
typealias BInner = B.Inner
```

Type aliases do not introduce new types. They are equivalent to the corresponding underlying types. When you add `typealias Predicate<T>` and use `Predicate<Int>` in your code, the Kotlin compiler always expands it to `(Int) -> Boolean`. Thus you can pass a variable of your type whenever a general function type is required and vice versa:
```
typealias Predicate<T> = (T) -> Boolean

fun foo(p: Predicate<Int>) = p(42)

fun main() {
    val f: (Int) -> Boolean = { it > 0 }
    println(foo(f)) // prints "true"

    val p: Predicate<Int> = { it > 0 }
    println(listOf(1, -2).filter(p)) // prints "[1]"
}
```

## [Why Use Type Aliases?](https://medium.com/@agayevrauf/type-aliases-in-kotlin-with-code-examples-945c3351c940#:~:text=Why%20Use%20Type%20Aliases%3F)
- **Improved Readability**: Type aliases can make your code more understandable by giving meaningful names to types that might otherwise be cryptic or long;
- **Abstraction and Encapsulation**: Type aliases can abstract away implementation details, allowing you to change the underlying types without affecting the rest of your codebase;
- **Code Reusability**: Type aliases can promote code reusability by providing a clear and consistent way to represent certain concepts or data structures;
- **Domain-Specific Language (DSL) Creation**: Type aliases can help you create a DSL that is more aligned with your problem domain.

## [Examples of Type Aliases](https://medium.com/@agayevrauf/type-aliases-in-kotlin-with-code-examples-945c3351c940#:~:text=Examples%20of%20Type%20Aliases)

### [Example 1: Enhancing Function Signatures](https://medium.com/@agayevrauf/type-aliases-in-kotlin-with-code-examples-945c3351c940#:~:text=Example%201%3A%20Enhancing%20Function%20Signatures)
Consider a scenario where you’re working with coordinates in a 2D space. Instead of using plain `Pair<Double, Double>`, you can create a type alias for it:
```
typealias Point = Pair<Double, Double>

fun distance(p1: Point, p2: Point): Double {
    val dx = p1.first - p2.first
    val dy = p1.second - p2.second
    return Math.sqrt(dx * dx + dy * dy)
}
```

Now, your function signatures are more descriptive and intuitive:
```
val p1: Point = 3.0 to 4.0
val p2: Point = 6.0 to 8.0
val dist = distance(p1, p2)
```

### [Example 2: Creating DSL-like Syntax](https://medium.com/@agayevrauf/type-aliases-in-kotlin-with-code-examples-945c3351c940#:~:text=Example%202%3A%20Creating%20DSL%2Dlike%20Syntax)
Type aliases can also be used to create a DSL-like syntax. Let’s say you’re building a simple HTML DSL:
```
typealias Tag = StringBuilder.() -> Unit

fun html(block: Tag): String {
    val sb = StringBuilder()
    sb.append("<html>")
    sb.block()
    sb.append("</html>")
    return sb.toString()
}

fun Tag.head(block: Tag) {
    append("<head>")
    block()
    append("</head>")
}

fun Tag.body(block: Tag) {
    append("<body>")
    block()
    append("</body>")
}

fun Tag.p(content: String) {
    append("<p>$content</p>")
}
```

With these type aliases, you can create HTML structures in a concise and intuitive way:
```
val page = html {
    head {
        p("Type Aliases in Kotlin")
    }
    body {
        p("Learn about type aliases.")
    }
}

println(page)
```

## [Example 3: Semantic Type Names](https://medium.com/@agayevrauf/type-aliases-in-kotlin-with-code-examples-945c3351c940#:~:text=Example%203%3A%20Semantic%20Type%20Names)
When working with APIs that use generic types, type aliases can give more semantic meaning to those types. For instance, when dealing with a list of user IDs:
```
typealias UserId = String

fun fetchUserProfile(userId: UserId): UserProfile {
    // Fetch user profile using the ID
}
```

Here, `UserId` makes the intent clearer than using a plain `String`.

### [Understanding the Limitations of Type Aliases](https://verbosemode.substack.com/p/kotlin-type-aliases-enhancing-code#:~:text=Understanding%20the%20Limitations%20of%20Type%20Aliases)
Type aliases in Kotlin are a valuable asset, enhancing the expressiveness and clarity of our code. They contribute significantly to both readability and code organization, showcasing simplicity at its best.

However, it's pivotal to understand their limitations. While they grant a new name to an existing type, they don't forge a new type. For instance, `typealias Email = String` aids in readability but doesn't bolster type safety. To the compiler, it remains a string, open to any string value.

To summarize the **limitations** of type aliases 📌:
- **Don't Create New Types**: They provide alternative names, not distinct types;
- **No Enhanced Type Safety**: For example, any string can fit into `typealias Email = String`;
- **Same Compiler Perception**: The compiler interprets them as their original types.

# Links
[Type aliases﻿](https://kotlinlang.org/docs/type-aliases.html)

[Type Aliases in Kotlin with Code Examples](https://medium.com/@agayevrauf/type-aliases-in-kotlin-with-code-examples-945c3351c940)

[Kotlin Type Aliases: Enhancing Code Clarity - Wednesday's Kotlin Kuppa](https://verbosemode.substack.com/p/kotlin-type-aliases-enhancing-code)

# Further reading
[All About Type Aliases in Kotlin](https://typealias.com/guides/all-about-type-aliases/)
