# val mutableList vs var immutableList

Mutable and immutable list increase the design clarity of the model. This is to force developer to think and clarify the purpose of collection.

- If the collection will change as part of design, use mutable collection
- If model is meant only for viewing, use immutable list

Purpose of `val` and `var` is how reference will work with variable:
- `var` - value/reference can be changed at any point of time
- `val` - value/reference can be assigned only once and can't be changed later

There are several reasons why immutable objects are often preferable:

-  They encourage functional programming, where state is not mutated, but passed on to the next function which creates a new state based on it. This is very well visible in the Kotlin collection methods such as `map`, `filter`, `reduce`, etc.
- A program without side effects is often easier to understand and debug
- In multithreaded programs, immutable resources cannot cause race conditions, as no write access is involved.

Immutable objects also have some disadvantages:
- Copying entire collections just to add/remove a single element is computationally expensive.
- In some cases, immutability can make the code more complex, when you tediously need to change single fields. In Kotlin, data classes come with a built-in `copy()` method where you can copy an instance, while providing new values for only some of the fields.

Example:

```
val mutableList = mutableListOf<Int>()
var immutableList = listOf<Int>()

fun testFunction() {
    
    //Can do
    mutableList.add(1)
    mutableList.add(2)
    
    //Can't do (val cannot be reassigned)
    val secondMutableList = mutableListOf<Int>()
    mutableList = secondMutableList
    
    //Can do
    val secondImmutableList = listOf<Int>()
    immutableList = secondImmutableList
    
    //Can't do 
    immutableList.add(1)
}
```

## Links
https://stackoverflow.com/questions/51718229/kotlin-val-mutablelist-vs-var-immutablelist-when-to-use-which  
