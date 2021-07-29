# What is the difference between “*” and “Any” in generics
The type `MutableList<*> `represents the list of something (you don't know what exactly). So if you try to add something to this list, you won't succeed. It may be a list of `Strings`, or a list of `Int`s, or a list of something else. The compiler won't allow to put any object in this list at all because it cannot verify that the list accepts objects of this type. However, if you attempt to get an element out of such list, you'll surely get an object of type `Any?`, because all objects in Kotlin inherit from `Any`.

Additionally `List<*>` can contain objects of any type, but only that type, so it can contain `Strings` (but only Strings), while `List<Any>` can contain Strings and Integers and whatnot, all in the same list.

## Example
Let's say that you have this generic class `Crate` that you intend to use for storing fruits. This class is invariant in `T`. This means, this class can consume as well as produce `T` (fruits). In other words, this class has functions that take `T` as argument(consume) as well as return `T`(produce). The `size()` function is `T`-independent, it neither takes `T` nor does it return `T`:

```
class Crate<T> {
    private val items = mutableListOf<T>()
    fun produce(): T = items.last()
    fun consume(item: T) = items.add(item)
    fun size(): Int = items.size
}
```

### Star projection is no producer, no consumer of `T`
By projecting the `Crate` as `*`, you are telling the compiler: "give me an error when I accidentally use the `Crate` class as a producer or consumer of `T` because I don't want to use the functions or properties that consume and produce `T`. I just want to safely use the `T`-independent functions and properties like `size()`":

```
fun useAsStar(star: Crate<*>) {

    // T is unknown, so the star produces the default supertype Any?.
    val anyNullable = star.produce()         // Not useful

    // T is unknown, cannot access its properties and functions.
    anyNullable.getColor()                   // Error

    // Cannot consume because you don't know the type of Crate.
    star.consume(Fruit())                    // Error

    // Only use the T-independent functions and properties.
    star.size()                              // OK
}
```

### `Any` is not a projection
When you say `Crate<Any>`, you are not projecting, you are simply using the original invariant class `Crate<T>` as it is, which can produce as well as consume `Any`:
```
fun useAsAny(any: Crate<Any>) {

    // T is known to be Any. So, an invariant produces Any.
    val anyNonNull = any.produce()           // OK

    // T is known to be Any. So, an invariant consumes Any.
    any.consume(Fruit())                     // OK

    // Can use the T-independent functions and properties, of course.
    any.size()                               // OK
}
```

# Links
[Difference between “*” and “Any” in Kotlin generics](https://stackoverflow.com/questions/40138923/difference-between-and-any-in-kotlin-generics)

# Further reading
[Generics: in, out, where](https://kotlinlang.org/docs/generics.html)