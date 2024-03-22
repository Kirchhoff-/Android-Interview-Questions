# Operator overloading
Kotlin allows you to provide custom implementations for the predefined set of operators on types. These operators have predefined symbolic representation (like `+` or `*`) and precedence. To implement an operator, provide a [member function](https://kotlinlang.org/docs/functions.html#member-functions) or an [extension function](https://kotlinlang.org/docs/extensions.html) with a specific name for the corresponding type. This type becomes the left-hand side type for binary operations and the argument type for the unary ones.<sup>[1](https://kotlinlang.org/docs/operator-overloading.html#unary-operations:~:text=Kotlin%20allows%20you,the%20unary%20ones.)</sup>

To overload an operator, mark the corresponding function with the `operator` modifier:
```
interface IndexedContainer {
    operator fun get(index: Int)
}
```

When overriding your operator overloads, you can omit `operator`:
```
class OrdersList: IndexedContainer {
    override fun get(index: Int) { /*...*/ }
}
```

The next operators can be overloaded: 
- [Unary prefix operators﻿](#Unary-prefix-operators);
- [Increments and decrements](#Increments-and-decrements);
- [Arithmetic operators﻿](#arithmetic-operators);
- [Indexed access operator﻿](#Indexed-access-operator);
- [Invoke operator﻿](#Invoke-operator);
- [Augmented assignments﻿](#Augmented-assignments);
- [Equality and inequality operators﻿](#Equality-and-inequality-operators);
- [Comparison operators﻿](#Comparison-operators).

## [Unary prefix operators﻿](https://kotlinlang.org/docs/operator-overloading.html#unary-prefix-operators)
| Expression  | Translated to  | 
|---|---|
| `+a`  | `a.unaryPlus()`  |
| `-a`  | `a.unaryMinus()` |
| `!a`  | `a.not()` |

This table says that when the compiler processes, for example, an expression `+a`, it performs the following steps:
- Determines the type of `a`, let it be `T`;
- Looks up a function `unaryPlus()` with the operator modifier and no parameters for the receiver `T`, that means a member function or an extension function;
- If the function is absent or ambiguous, it is a compilation error;
- If the function is present and its return type is `R`, the expression `+a` has type `R`.

As an example, here's how you can overload the unary minus operator:
```
data class Point(val x: Int, val y: Int)

operator fun Point.unaryMinus() = Point(-x, -y)

val point = Point(10, 20)

fun main() {
   println(-point)  // prints "Point(x=-10, y=-20)"
}
```

## [Increments and decrements﻿](https://kotlinlang.org/docs/operator-overloading.html#increments-and-decrements)
| Expression  | Translated to  | 
|---|---|
| `a++`  | `a.inc()` |
| `a--`  | `a.dec()` |

The `inc()` and `dec()` functions must return a value, which will be assigned to the variable on which the `++` or `--` operation was used. They shouldn't mutate the object on which the `inc` or `dec` was invoked.

The compiler performs the following steps for resolution of an operator in the **postfix** form, for example `a++`:
- Determines the type of `a`, let it be `T`;
- Looks up a function `inc()` with the `operator` modifier and no parameters, applicable to the receiver of type `T`;
- Checks that the return type of the function is a subtype of `T`.

## [Arithmetic operators﻿](https://kotlinlang.org/docs/operator-overloading.html#arithmetic-operators)
| Expression  | Translated to  | 
|---|---|
| `a + b`  | `a.plus(b)` |
| `a - b`  | `a.minus(b)` |
| `a * b`  | `a.times(b)` |
| `a / b`  | `a.div(b)` |
| `a % b`  | `a.rem(b)` |
| `a..b`   | `a.rangeTo(b)` |
| `a..<b`  | `a.rangeUntil(b)` |

For the operations in this table, the compiler just resolves the expression in the **Translated to** column.

Example:<sup>[2](https://www.programiz.com/kotlin-programming/operator-overloading#:~:text=Example%3A%20Overloading%20%2B%20Operator)</sup>
```
fun main(args: Array<String>) {
    val p1 = Point(3, -8)
    val p2 = Point(2, 9)

    var sum = Point()
    sum = p1 + p2

    println("sum = (${sum.x}, ${sum.y})")
}

class Point(val x: Int = 0, val y: Int = 10) {

    // overloading plus function
    operator fun plus(p: Point) : Point {
        return Point(x + p.x, y + p.y)
    }
}
```

When you run the program, the output will be:
```
sum = (5, 1)
```

## [Indexed access operator﻿](https://kotlinlang.org/docs/operator-overloading.html#indexed-access-operator)
| Expression  | Translated to  | 
|---|---|
| `a[i]`  | `a.get(i)`  |
| `a[i, j]`  | `a.get(i, j)`  |
| `a[i_1, ..., i_n]`  | `a.get(i_1, ..., i_n)`  |
| `a[i] = b`  | `a.set(i, b)`  |
| `a[i, j] = b`  | `a.set(i, j, b)`  |
| `a[i_1, ..., i_n] = b`  | `a.set(i_1, ..., i_n, b)`  |

Square brackets are translated to calls to `get` and `set` with appropriate numbers of arguments.

## [Invoke operator](https://kotlinlang.org/docs/operator-overloading.html#invoke-operator)
| Expression  | Translated to  | 
|---|---|
| `a()`  | `a.invoke()`  |
| `a(i)`  | `a.invoke(i)`  |
| `a(i, j)`  | `a.invoke(i, j)`  |
|  `a(i_1, ..., i_n)` | `a.invoke(i_1, ..., i_n)`  |

Parentheses are translated to calls to `invoke` with appropriate number of arguments.

Specifying an invoke operator on a class allows it to be called on *any instances of the class without a method name*.<sup>[3](https://stackoverflow.com/questions/45173677/invoke-operator-operator-overloading-in-kotlin#:~:text=Specifying%20an%20invoke%20operator%20on%20a%20class%20allows%20it%20to%20be%20called%20on%20any%20instances%20of%20the%20class%20without%20a%20method%20name.)</sup>

Let’s see this in action:
```
class Greeter(val greeting: String) {
    operator fun invoke(name: String) {
        println("$greeting $name")
    }
}

fun main(args: Array<String>) {
    val greeter = Greeter(greeting = "Welcome")
    greeter(name = "Kotlin")
    //this calls the invoke function which takes String as a parameter
}
```

## [Augmented assignments﻿](https://kotlinlang.org/docs/operator-overloading.html#augmented-assignments)
| Expression  | Translated to  | 
|---|---|
| `a += b`  | `a.plusAssign(b)`  |
| `a -= b`  | `a.minusAssign(b)`  |
| `a *= b`  | `a.timesAssign(b)`  |
| `a /= b`  | `a.divAssign(b)`  |
| `a %= b`  | `a.remAssign(b)`  |

For the assignment operations, for example `a += b`, the compiler performs the following steps:
- If the function from the right column is available:
  - If the corresponding binary function (that means `plus()` for `plusAssign()`) is available too, `a` is a mutable variable, and the return type of `plus` is a subtype of the type of `a`, report an error (ambiguity);
  - Make sure its return type is `Unit`, and report an error otherwise;
  - Generate code for `a.plusAssign(b)`.
- Otherwise, try to generate code for `a = a + b` (this includes a type check: the type of `a + b` must be a subtype of `a`).

## [Equality and inequality operators﻿](https://kotlinlang.org/docs/operator-overloading.html#equality-and-inequality-operators)
| Expression  | Translated to  | 
|---|---|
| `a == b`  | `a?.equals(b) ?: (b === null)`  |
| `a != b`  | `!(a?.equals(b) ?: (b === null))`  |

These operators only work with the function `equals(other: Any?): Boolean`, which can be overridden to provide custom equality check implementation. Any other function with the same name (like `equals(other: Foo)`) will not be called.

## [Comparison operators﻿](https://kotlinlang.org/docs/operator-overloading.html#comparison-operators)
| Expression  | Translated to  | 
|---|---|
| `a > b`  | `a.compareTo(b) > 0`  |
| `a < b`  | `a.compareTo(b) < 0`  |
| `a >= b`  | `a.compareTo(b) >= 0`  |
| `a <= b`  | `a.compareTo(b) <= 0`  |

All comparisons are translated into calls to `compareTo`, that is required to return `Int`.

# Links
[Operator overloading﻿](https://kotlinlang.org/docs/operator-overloading.html)

[Kotlin Operator Overloading](https://www.programiz.com/kotlin-programming/operator-overloading)

[Invoke Operator & Operator Overloading in Kotlin](https://stackoverflow.com/questions/45173677/invoke-operator-operator-overloading-in-kotlin)

# Further reading
[Operator overloading in Kotlin](https://kt.academy/article/kfde-operators)

[Conventions & Operator Overloading](https://www.kodeco.com/books/kotlin-apprentice/v3.0/chapters/22-conventions-operator-overloading)

[How Can Kotlin Operator Overloading Help You?](https://codersee.com/how-can-kotlin-operator-overloading-help-you/)

[Operator overloading](https://kotlinlang.org/spec/operator-overloading.html)

[Mastering Kotlin indexed access operator](https://www.manhhavu.com/posts/mastering-kotlin-indexed-access-operator/)
