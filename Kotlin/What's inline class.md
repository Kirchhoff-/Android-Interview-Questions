# Inline class

Sometimes it is necessary for business logic to create a wrapper around some type. However, it introduces runtime overhead due to additional heap allocations. Moreover, if the wrapped type is primitive, the performance hit is terrible, because primitive types are usually heavily optimized by the runtime, while their wrappers don't get any special treatment.

To solve such issues, Kotlin introduces a special kind of class called an inline class. 
Inline classes add a simple tool we can use to wrap some other type without adding runtime overhead through additional heap allocations.

Inline classes are pretty straightforward. To make a class inlined, you just add the inline keyword to your class:

`inline class WrappedInt(val value: Int)`

Restriction of inline classes:
- must have a single property initialized in the primary constructor
- cannot have init blocks
- inline class properties cannot have backing fields
- can only have simple computable properties (no lateinit/delegated properties)
- cannot extend other classes and must be final.

## Type Aliases vs. Inline Classes

At first sight, inline classes may appear to be very similar to type aliases. Indeed, both seem to introduce a new type and both will be represented as the underlying type at runtime.
However, the crucial difference is that type aliases are assignment-compatible with their underlying type (and with other type aliases with the same underlying type), while inline classes are not.
In other words, inline classes introduce a truly new type, contrary to type aliases which only introduce an alternative name (alias) for an existing type

## Links
https://kotlinlang.org/docs/reference/inline-classes.html   
https://kotlinexpertise.com/kotlin-inline-classes/  
https://typealias.com/guides/introduction-to-inline-classes/  
