# Ranges
Range is a collection of finite values which is defined by endpoints. The range in Kotlin consists of a start, a stop, and the step. The start and stop are inclusive in the Range and the value of step is by default 1. The range is used with comparable types. 

Example of range: 
```
if (i in 1..4) {  // equivalent of 1 <= i && i <= 4
    print(i)
}
```

Integral type ranges (`IntRange`, `LongRange`, `CharRange`) have an extra feature: they can be iterated over. Such ranges are generally used for iteration in the `for` loops.

```
for (i in 1..4) print(i)
```

To iterate numbers in reverse order, use the `downTo` function instead of `...`
```
for (i in 4 downTo 1) print(i)
```

It is also possible to iterate over numbers with an arbitrary step (not necessarily 1). This is done via the `step` function.
```
for (i in 1..8 step 2) print(i)
println()
for (i in 8 downTo 1 step 2) print(i)
```

To iterate a number range which does not include its end element, use the `until` function:
```
for (i in 1 until 10) {       // i in [1, 10), 10 is excluded
    print(i)
}
```

There are three ways for creating Range in Kotlin –
- Using `(..)` operator
- Using `rangeTo()` function
- Using `downTo()` function

`(..)` It is the simplest way to work with range. It will create a range from the start to end including both the values of start and end. It is the operator form of `rangeTo()` function. Using `(..)` operator we can create range for integers as well as characters. Example of using `(..)` range operator:
```
fun main(args : Array<String>){ 
  
    println("Integer range:") 
    // creating integer range  
    for(num in 1..5){ 
        println(num) 
    } 
} 
```

Output:
```
Integer range:
1
2
3
4
5
```
`rangeTo()` it is similar to `(..)` operator. It will create a range upto the value passed as an argument. It is also used to create range for integers as well as characters. Example of using `rangeTo()` function:

```
fun main(args : Array<String>){ 
  
    println("Integer range:") 
    // creating integer range  
    for(num in 1.rangeTo(5)){ 
        println(num) 
    } 
} 
```

Output:
```
Integer range:
1
2
3
4
5
```

`downTo()` It is reverse of the `rangeTo()` or `(..)` operator. It creates a range in descending order, i.e. from bigger values to smaller value. Example of using `downTo()` function:
```
fun main(args : Array<String>){ 
  
    println("Integer range in descending order:") 
    // creating integer range 
    for(num in 5.downTo(1)){ 
        println(num) 
    } 
} 
```

`step()` function 
With keyword step, one can provide step between values. It is mainly used in order to provide the gap between the two values in `rangeTo()` or `downTo()` or in `(..)` operator. The default value for step is 1 and the value of step function cannot be 0.

```
fun  main(args: Array<String>) { 
    //for iterating over the range 
    var i = 2
    // for loop with step keyword 
    for (i in 3..10 step 2)  
        print("$i ")  
    println() 
    // print first value of the range 
    println((11..20 step 2).first)  
    // print last value of the range 
    println((11..20 step 4).last)   
    // print the step used in the range 
    println((11..20 step 5).step)   
} 
```

Output:
```
3 5 7 9 
11
19
5
```

## Links
https://kotlinlang.org/docs/reference/ranges.html  
https://www.baeldung.com/kotlin-ranges  
https://www.geeksforgeeks.org/kotlin-ranges/  
