# Sequence
Sequences offer the same functions as `Iterable` but implement another approach to multi-step collection processing.

When the processing of an `Iterable` includes multiple steps, they are executed eagerly: each processing step completes and returns its result – an intermediate collection. The following step executes on this collection. In turn, multi-step processing of sequences is executed lazily when possible: actual computing happens only when the result of the whole processing chain is requested.

The order of operations execution is different as well: `Sequence` performs all the processing steps one-by-one for every single element. In turn, `Iterable` completes each step for the whole collection and then proceeds to the next step.

So, the sequences let you avoid building results of intermediate steps, therefore improving the performance of the whole collection processing chain. However, the lazy nature of sequences adds some overhead which may be significant when processing smaller collections or doing simpler computations. Hence, you should consider both `Sequence` and `Iterable` and decide which one is better for your case.

## Sequence operations
The sequence operations can be classified into the following groups regarding their state requirements:
- *Stateless* operations require no state and process each element independently, for example, map() or filter(). Stateless operations can also require a small constant amount of state to process an element, for example, take() or drop().
- *Stateful* operations require a significant amount of state, usually proportional to the number of elements in a sequence.

If a sequence operation returns another sequence, which is produced lazily, it's called `intermediate`. Otherwise, the operation is `terminal`. Examples of terminal operations are `toList()` or `sum()`. Sequence elements can be retrieved only with terminal operations.

Sequences can be iterated multiple times; however some sequence implementations might constrain themselves to be iterated only once. That is mentioned specifically in their documentation.

Example: 
```
sequenceOf("A", "B", "C")
    .filter {
        println("filter: $it")
        true
    }
    .forEach {
        println("forEach: $it")
    }
    
Output:
// filter:  A
// forEach: A
// filter:  B
// forEach: B
// filter:  C
// forEach: C
```

## Links
https://kotlinlang.org/docs/reference/sequences.html  
https://typealias.com/guides/when-to-use-sequences/  
https://medium.com/androiddevelopers/collections-and-sequences-in-kotlin-55db18283aca  
https://winterbe.com/posts/2018/07/23/kotlin-sequence-tutorial/  
