# Power of Three
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)

Given an integer `n`, determine whether it is a power of three.

A number is considered a power of three if it can be written as `3^x`, where `x` is a non-negative integer.

For example:

* `1 = 3^0`
* `3 = 3^1`
* `9 = 3^2`
* `27 = 3^3`

Return `true` if `n` is a power of three. Otherwise, return `false`.

### Constraints

* `-2^31 <= n <= 2^31 - 1`

## Solution 1. Repeated Division

Unlike the [`Power of Two`](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Power%20of%20Two.md) problem, there is no bit manipulation trick that can help us here.

Powers of two have a very specific binary representation. Each valid number contains exactly one set bit:

```text
1  = 1
2  = 10
4  = 100
8  = 1000
16 = 10000
```

Because of that, we can use bitwise operations to solve the problem.

Powers of three look completely different in binary:

```text
1  = 1
3  = 11
9  = 1001
27 = 11011
81 = 1010001
```

There is no obvious pattern that we can take advantage of. So let's put the clever tricks aside for a moment and start with the most straightforward solution.

The most straightforward solution is to keep dividing the number by `3` while it is divisible by `3`. If we eventually reach `1`, then the original number was a power of three. Otherwise, it was not.

```kotlin
fun isPowerOfThree(n: Int): Boolean {
    if (n <= 0) {
        return false
    }

    var number = n

    while (number % 3 == 0) {
        number /= 3
    }

    return number == 1
}
```

For example, if `n = 27`:

```text
27 / 3 = 9
9 / 3 = 3
3 / 3 = 1
```

Since we reached `1`, the answer is `true`.

If `n = 45`:

```text
45 / 3 = 15
15 / 3 = 5
```

The division stops at `5`, so the answer is `false`.

**Complexity**

Time complexity: `O(log₃ n)`

Space complexity: `O(1)`

## Solution 2. Enumerate All Possible Answers

Now let's look at the constraints more carefully.

```text
-2^31 <= n <= 2^31 - 1
```

The important detail here is that the input is limited to `Int`. This means there is only a finite number of powers of three that can fit into the valid range.

If we calculate them, we will eventually reach:

```text
3^19 = 1162261467
```

The next value:

```text
3^20 = 3486784401
```

is already larger than `Int.MAX_VALUE`.

There are only twenty valid powers of three. That's not exactly a tiny list, but it's small enough that we could simply enumerate all of them.

```kotlin
private val POWERS_OF_THREE = setOf(
    1,
    3,
    9,
    27,
    81,
    243,
    729,
    2187,
    6561,
    19683,
    59049,
    177147,
    531441,
    1594323,
    4782969,
    14348907,
    43046721,
    129140163,
    387420489,
    1162261467
)

fun isPowerOfThree(n: Int): Boolean = n in POWERS_OF_THREE
```

This solution is fast, simple, and would pass all test cases. However, it also feels a bit like cheating. Instead of solving the problem, we are simply memorizing all possible answers.

**Complexity**

Time complexity: `O(1)`

Space complexity: `O(1)`

## Solution 3. Mathematical Trick

Let's look again at the largest power of three that fits into an `Int`:

```text
3^19 = 1162261467
```

Now let's ask a different question:

> What happens if we divide this number by another power of three?

For example:

```text
3^19 / 3^10 = 3^9
3^19 / 3^5 = 3^14
3^19 / 3^0 = 3^19
```

The result is always another power of three, which means every valid power of three divides `3^19` without a remainder.

This happens because:

```text
3^19 / 3^k = 3^(19 - k)
```

and the result is always an integer.

Because of that, if a positive number is a power of three, then:

```text
1162261467 % n == 0
```

must be true.

This gives us a surprisingly simple solution:

```kotlin
private const val MAX_POWER_OF_THREE = 1_162_261_467

fun isPowerOfThree(n: Int): Boolean =
    n > 0 && MAX_POWER_OF_THREE % n == 0
```

**Complexity**

Time complexity: `O(1)`

Space complexity: `O(1)`

## Alternative Implementation

In the previous solution, we used a hardcoded value:

```kotlin
private const val MAX_POWER_OF_THREE = 1_162_261_467
```

This works perfectly, but it may not always be obvious where this number comes from.

If needed, we can calculate it directly:

```kotlin
fun isPowerOfThree(n: Int): Boolean =
    n > 0 && 3.0.pow(19).toInt() % n == 0
```

This produces the same result because:

```text
3^19 = 1162261467
```

However, in practice, using a named constant is usually preferred because it avoids recalculating the value and makes the intent more explicit.

**Complexity**

Time complexity: `O(1)`

Space complexity: `O(1)`
