# Property references and `::` operator
The `::` operator is known as the "member reference" or "callable reference" operator. It can be used to create a references to the following:
- Class reference `val myClass = MyClass::class`;
- Function reference `this::isEmpty`, `::isOdd`;
- Property reference `::someVal.isInitialized`;
- Constructor reference `::MyClass`.<sup>[1](https://stackoverflow.com/questions/47400942/what-does-mean-in-kotlin#:~:text=Class%20Reference%20val,Reference%20%3A%3AMyClass)</sup>

## [Class reference](https://kotlinlang.org/docs/reflection.html#class-references)
To obtain the reference to a statically known Kotlin class, you can use the **class literal** syntax:
```
val c = MyClass::class
```

The reference is a [`KClass`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.reflect/-k-class/) type value.

## [Function reference](https://kotlinlang.org/docs/reflection.html#function-references)
When you have a named function declared as below, you can call it directly (`isOdd(5)`):
```
fun isOdd(x: Int) = x % 2 != 0
```

Alternatively, you can use the function as a function type value, that is, pass it to another function. To do so, use the `::` operator:
```
val numbers = listOf(1, 2, 3)
println(numbers.filter(::isOdd))
```

Here `::isOdd` is a value of function type `(Int) -> Boolean`.

Function references belong to one of the [`KFunction<out R>`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.reflect/-k-function/) subtypes, depending on the parameter count.

One of the most common use cases for function reference is passing it as a parameter to a higher-order function. Higher-order functions are functions that take other functions as parameters or return functions as results. By passing a member function as a parameter, you can reuse the same functionality across different contexts:
```
class MyClass {
    fun myFunction(param: Int) {
        // function implementation
    }
}

fun higherOrderFunction(func: (Int) -> Unit) {
    // do something
}

fun main() {
    val myClassInstance = MyClass()
    higherOrderFunction(myClassInstance::myFunction)
}
```

In this example, we have a class called `MyClass` with a member function called `myFunction`. We then create an instance of `MyClass` and store it in the `myClassInstance` variable. Finally, we pass a reference to the `myFunction` function using member reference to the `higherOrderFunction`, which takes a function with a single `Int` parameter and returns nothing.<sup>[2](https://medium.com/softaai-blogs/member-reference-in-kotlin-3da936472840#:~:text=Passing%20a%20member,returns%20nothing.)</sup>

## [Property reference](https://kotlinlang.org/docs/reflection.html#property-references)
To access properties as first-class objects in Kotlin, use the `::` operator:
```
val x = 1

fun main() {
    println(::x.get())
    println(::x.name)
}
```

The expression `::x` evaluates to a `KProperty0<Int>` type property object. You can read its value using `get()` or retrieve the property name using the `name` property.

A property reference can be used where a function with a single generic parameter is expected:
```
val strs = listOf("a", "bc", "def")
println(strs.map(String::length))
```

To access a property that is a member of a class, qualify it as follows:
```
class A(val p: Int)
val prop = A::p
println(prop.get(A(1)))
```

For an extension property:
```
val String.lastChar: Char
    get() = this[length - 1]

fun main() {
    println(String::lastChar.get("abc"))
}
```

## [Constructor reference](https://kotlinlang.org/docs/reflection.html#constructor-references)
Constructors can be referenced just like methods and properties. You can use them wherever the program expects a function type object that takes the same parameters as the constructor and returns an object of the appropriate type. Constructors are referenced by using the `::` operator and adding the class name. Consider the following function that expects a function parameter with no parameters and return type `Foo`:
```
class Foo

fun function(factory: () -> Foo) {
    val x: Foo = factory()
}
```

Using `::Foo`, the zero-argument constructor of the class `Foo`, you can call it like this:
```
function(::Foo)
```

Callable references to constructors are typed as one of the [`KFunction<out R>`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.reflect/-k-function/) subtypes depending on the parameter count.

## [Bound function and property references﻿](https://kotlinlang.org/docs/reflection.html#bound-function-and-property-references)
You can refer to an instance method of a particular object:
```
val numberRegex = "\\d+".toRegex()
println(numberRegex.matches("29"))

val isNumber = numberRegex::matches
println(isNumber("29"))
```

Instead of calling the method `matches` directly, the example uses a reference to it. Such a reference is bound to its receiver. It can be called directly (like in the example above) or used whenever a function type expression is expected:
```
val numberRegex = "\\d+".toRegex()
val strings = listOf("abc", "124", "a70")
println(strings.filter(numberRegex::matches))
```

Compare the types of the bound and the unbound references. The bound callable reference has its receiver "attached" to it, so the type of the receiver is no longer a parameter:
```
val isNumber: (CharSequence) -> Boolean = numberRegex::matches

val matches: (Regex, CharSequence) -> Boolean = Regex::matches
```

A property reference can be bound as well:
```
val prop = "abc"::length
println(prop.get())
```

You don't need to specify `this` as the receiver: `this::foo` and `::foo` are equivalent.

# Links
[what does "::" mean in kotlin?](https://stackoverflow.com/questions/47400942/what-does-mean-in-kotlin)

[Reflection﻿](https://kotlinlang.org/docs/reflection.html)

[Member Reference in Kotlin](https://medium.com/softaai-blogs/member-reference-in-kotlin-3da936472840)

# Further reading 
[Kotlin Reflection: Method and property references](https://kt.academy/article/ak-reflection)

[Internally mechanism of callable operator :: in kotlin — Byte code level](https://blog.devgenius.io/internally-mechanism-of-callable-operator-in-kotlin-byte-code-level-c683c73e78fd)

[Understanding the :: Operator in Kotlin: The Callable Reference Operator](https://blog.stackademic.com/understanding-the-operator-in-kotlin-the-callable-reference-operator-057ae42b4a6b)
