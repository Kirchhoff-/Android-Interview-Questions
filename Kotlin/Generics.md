# Generics
Generic programming is a way of writing our code in a flexible manner like we would in a dynamically-typed language. At the same time, generics allow us to write code safely and with as few compile-time errors as possible.

Using generics in Kotlin enables the developer to focus on creating reusable solutions, or templates, for a wider range of problems.

## Generic class
Classes in Kotlin can have type parameters, just like in Java:
```
class Box<T>(t: T) {
    var value = t
}
```

To create an instance of such a class, simply provide the type arguments:
```
val box: Box<Int> = Box<Int>(1)
```

But if the parameters can be inferred, for example, from the constructor arguments, you can omit the type arguments:
```
val box = Box(1) // 1 has type Int, so the compiler figures out that it is Box<Int>
```

## [Generic functions](https://kotlinlang.org/docs/generics.html#generic-functions)
Classes aren't the only declarations that can have type parameters. Functions can, too. Type parameters are placed *before* the name of the function:
```
fun <T> singletonList(item: T): List<T> {
    // ...
}

fun <T> T.basicToString(): String { // extension function
    // ...
}
```

To call a generic function, specify the type arguments at the call site *after* the name of the function:
```
val l = singletonList<Int>(1)
```

Type arguments can be omitted if they can be inferred from the context, so the following example works as well:
```
val l = singletonList(1)
```

## [Generic constraints](https://kotlinlang.org/docs/generics.html#generic-constraints)
The set of all possible types that can be substituted for a given type parameter may be restricted by *generic constraints*.

### [Upper bounds](https://kotlinlang.org/docs/generics.html#upper-bounds)
The most common type of constraint is an *upper bound*, which corresponds to Java's `extends` keyword:
```
fun <T : Comparable<T>> sort(list: List<T>) {  ... }
```

The type specified after a colon is the *upper bound*, indicating that only a subtype of `Comparable<T>` can be substituted for `T`. For example:
```
sort(listOf(1, 2, 3)) // OK. Int is a subtype of Comparable<Int>
sort(listOf(HashMap<Int, String>())) // Error: HashMap<Int, String> is not a subtype of Comparable<HashMap<Int, String>>
```

The default upper bound (if there was none specified) is `Any?`. Only one upper bound can be specified inside the angle brackets. If the same type parameter needs more than one upper bound, you need a separate *where-clause*:
```
fun <T> copyWhenGreater(list: List<T>, threshold: T): List<String>
    where T : CharSequence,
          T : Comparable<T> {
    return list.filter { it > threshold }.map { it.toString() }
}
```

The passed type must satisfy all conditions of the `where` clause simultaneously. In the above example, the `T` type must implement *both* `CharSequence` and `Comparable`.

## [Type erasure](https://kotlinlang.org/docs/generics.html#type-erasure)
The type safety checks that Kotlin performs for generic declaration usages are done at compile time. At runtime, the instances of generic types do not hold any information about their actual type arguments. The type information is said to be *erased*. For example, the instances of `Foo<Bar>` and `Foo<Baz?>` are erased to just `Foo<*>`.

### [Generics type checks and casts](https://kotlinlang.org/docs/generics.html#generics-type-checks-and-casts)
Due to the type erasure, there is no general way to check whether an instance of a generic type was created with certain type arguments at runtime, and the compiler prohibits such `is` - checks such as `ints is List<Int>` or `list is T` (type parameter). However, you can check an instance against a star-projected type:
```
if (something is List<*>) {
    something.forEach { println(it) } // The items are typed as `Any?`
}
```

Similarly, when you already have the type arguments of an instance checked statically (at compile time), you can make an `is` - check or a cast that involves the non-generic part of the type. Note that angle brackets are omitted in this case:
```
fun handleStrings(list: MutableList<String>) {
    if (list is ArrayList) {
        // `list` is smart-cast to `ArrayList<String>`
    }
}
```

The same syntax but with the type arguments omitted can be used for casts that do not take type arguments into account: `list as ArrayList`.

### [Unchecked casts](https://kotlinlang.org/docs/generics.html#unchecked-casts)
Type casts to generic types with concrete type arguments such as `foo as List<String>` cannot be checked at runtime.

These unchecked casts can be used when type safety is implied by the high-level program logic but cannot be inferred directly by the compiler. See the example below.
```
fun readDictionary(file: File): Map<String, *> = file.inputStream().use {
    TODO("Read a mapping of strings to arbitrary elements.")
}

// We saved a map with `Int`s into this file
val intsFile = File("ints.dictionary")

// Warning: Unchecked cast: `Map<String, *>` to `Map<String, Int>`
val intsDictionary: Map<String, Int> = readDictionary(intsFile) as Map<String, Int>
```

A warning appears for the cast in the last line. The compiler can't fully check it at runtime and provides no guarantee that the values in the map are `Int`.

To avoid unchecked casts, you can redesign the program structure. In the example above, you could use the `DictionaryReader<T>` and `DictionaryWriter<T>` interfaces with type-safe implementations for different types. You can introduce reasonable abstractions to move unchecked casts from the call site to the implementation details. 

# Links
[Generics: in, out, where](https://kotlinlang.org/docs/generics.html)

[Understanding Kotlin generics](https://blog.logrocket.com/understanding-kotlin-generics/)

# Further reading
[Generics in Kotlin](https://kt.academy/article/kfde-generics)
