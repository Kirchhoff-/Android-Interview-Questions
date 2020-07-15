# Infix function
Kotlin allows some functions to be called without using the period and brackets. These are called infix methods, and their use can result in code that looks much more like a natural language. 

Infix functions must satisfy the following requirements:
- They must be member functions or extension functions;
- They must have a single parameter;
- The parameter must not accept variable number of arguments and must have no default value.

```
infix fun Int.shl(x: Int): Int { ... }

// calling the function using the infix notation
1 shl 2

// is the same as
1.shl(2)
```

Infix functions always require both the receiver and the parameter to be specified. When you're calling a method on the current receiver using the infix notation, you need to use `this` explicitly; unlike regular method calls, it cannot be omitted. This is required to ensure unambiguous parsing.

```
class MyStringCollection {
    infix fun add(s: String) { /*...*/ }
    
    fun build() {
        this add "abc"   // Correct
        add("abc")       // Correct
        //add "abc"      // Incorrect: the receiver must be specified
    }
}
```

One of the most common infix function is the `to` function to create a `Pair` object. It can be invoked in this way: 

`1 to "apple"` 

instead of:  

`1.to("apple")`. 

Also various numeric classes – `Byte`, `Short`, `Int`, and `Long` – all define the bitwise functions `and()`, `or()`, `shl()`, `shr()`, `ushr()`, and `xor()`, allowing some more readable expressions:

```
val color = 0x123456
val red = (color and 0xff0000) shr 16
val green = (color and 0x00ff00) shr 8
val blue = (color and 0x0000ff) shr 0
```
The `Boolean` class defines the `and()`, `or()` and `xor()` logical functions in a similar way:
```
if ((targetUser.isEnabled and !targetUser.isBlocked) or currentUser.admin) {
    // Do something if the current user is an Admin, or the target user is active
}
```

The `String` class also defines the match and `zip` functions as `infix`, allowing some simple-to-read code:

`"Hello, World" matches "^Hello".toRegex()`

**Note**: Infix function calls have lower precedence than the arithmetic operators, type casts, and the rangeTo operator. The following expressions are equivalent:
- `1 shl 2 + 3` is equivalent to `1 shl (2 + 3)`
- `0 until n * 2` is equivalent to `0 until (n * 2)`
- `xs union ys as Set<*>` is equivalent to `xs union (ys as Set<*>)`

On the other hand, infix function call's precedence is higher than that of the boolean operators `&&` and `||`, is- and in-checks, and some other operators. These expressions are equivalent as well:
- `a && b xor c` is equivalent to `a && (b xor c)`
- `a xor b in c` is equivalent to `(a xor b) in c`

## Links
https://kotlinlang.org/docs/reference/functions.html  
https://www.baeldung.com/kotlin-infix-functions  
https://medium.com/tompee/idiomatic-kotlin-infix-functions-eea833f70c90  
