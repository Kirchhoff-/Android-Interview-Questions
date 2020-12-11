### Reduce  
- `reduce` method transforms a given collection into a single result. It takes a lamda operator to combine a pair of elements into a accumulated value.  
It then traverses the collection from left to right and stepwise combines the accumulated value with the next element.  
  
        val numbers: List<Int> = listOf(1, 2, 3);  
        val sum = numbers.reduce { acc, next -> next + acc }  `

 -  `reduce` method will throw `RuntimeException` if its operated on an empty list.  
   
 - Signature of `reduce` method  
 `inline fun <S, T:S> Iterable<T>.reduce(operation: (acc:S, T) -> S): S`  
  The function defines a generic type S and subtype T of S, Finally reduce returns one value of type S.  
   
 - In the following example the sum would exceed  the range of `Int` because of that the data type of result is changed to Long. So it will throw compile error(Type argument is not within its bounds)  
 ` val sum: Long = numbers.reduce<Long, Int> { acc, next -> acc.toLong() + next.toLong() }`
   
  - To fix the compile error we will have to change the `Long` type to `Number` as `Number` is superset of `Int`.  So **It doesnt solve the problem of changing the result type**  
   
 ## Fold  
 To address the issues created by `reduce` we can use `fold`  

     `val numbers: List<Int> = listOf(1, 2, 3)  
      val sum = numbers.fold(0, { acc, next -> next + acc })`  

  Here first argument is initial value if list is empty the inital value is returned.  
 `inline fun <T, R> Iterable<T>.fold(initial:R, (acc:R, T) -> R): R`  
In contrast to `reduce()`, it specifies two arbitrary generic types **T** and **R**.
- We  can change the result type to **Long**  in the following example

    `val sum: Long = numbers.fold(0L, { acc, next -> acc + next.toLong() })`

 -  `fold` **provides the ability to change the result type**.
