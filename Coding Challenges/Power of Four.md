# Power of Four
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)

Given an integer `n`, determine whether it is a power of four.

A number is considered a power of four if it can be written as:

```kotlin
n = 4^x
```

where `x` is a non-negative integer. If such a value of `x` exists, return `true`. Otherwise, return `false`.

### Constraints

```kotlin
-2^31 <= n <= 2^31 - 1
```

## Solution 1. Repeated Division

Just like the [Power of Two](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Power%20of%20Two.md) and [Power of Three](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Power%20of%20Three.md) problems, we can solve this one using a brute-force approach.

The idea is simple: as long as the number is divisible by `4`, we keep dividing it by `4`. Eventually, one of two things will happen:

- We reach `1`, which means the original number was a power of four.
- We reach a number that is not divisible by `4`, which means the original number cannot be expressed as `4^x`.

```kotlin
fun isPowerOfFour(n: Int): Boolean {
    var number: Int = n

    while (number > 0 && number % 4 == 0) {
        number /= 4
    }

    return number == 1
}
```

**Complexity**

**Time complexity**: `O(log₄ n)`

Each iteration divides the number by `4`, so the number of iterations is equal to the number of times `n` can be divided by `4`.

**Space complexity**: `O(1)`

The algorithm uses only a single variable regardless of the input size.

## Solution 2. Kotlin Standard Library

This solution is very similar to the one used in the [Power of Two](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Power%20of%20Two.md) problem, but with one additional condition.

A power of four is always a power of two, so its binary representation contains exactly one `'1'` bit. However, not every power of two is a power of four:

```text
2  = 10
4  = 100
8  = 1000
16 = 10000
```

Notice that powers of four always have an **odd** number of bits in their binary representation (`1`, `3`, `5`, `7`, ...).

Using Kotlin standard library functions, we can convert the number to its binary representation and verify two conditions:

- The binary representation contains exactly one `'1'` bit.
- The length of the binary string is odd.

```kotlin
fun isPowerOfFour(n: Int): Boolean {
    if (n <= 0) {
        return false
    }

    val binary: String = n.toString(2)

    return binary.count { it == '1' } == 1 &&
        binary.length % 2 == 1
}
```

**Complexity**

**Time complexity**: `O(log n)`

Converting the number to its binary representation takes `O(log n)` time, where `log n` is the number of bits. Counting the `'1'` bits also requires traversing the binary string once, so the overall complexity remains `O(log n)`.

**Space complexity**: `O(log n)`

The binary string contains one character per bit, so it requires `O(log n)` additional space.

## Solution 3. Bit Manipulation Trick

From the previous solution, we already know two important facts:

- Every power of four is also a power of two.
- Powers of four always have an odd total number of bits in their binary representation.

The first condition can be verified exactly the same way as in the [Power of Two](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Power%20of%20Two.md) problem:

```kotlin
(n and (n - 1)) == 0
```

This guarantees that the number contains exactly one `1` bit.

The only thing left is to verify that this `1` bit is in the correct position. We can do that using the following bit mask:

```kotlin
0x55555555
```

In binary, it looks like this:

```text
01010101010101010101010101010101
```

Notice that every other bit is set to `1`. These are exactly the positions where the single `1` bit of a power of four may appear.

For example:

```text
4  = 00000100
16 = 00010000
64 = 01000000
```

Each of these numbers has its only `1` bit aligned with one of the `1` bits in the mask, so:

```kotlin
(n and 0x55555555) != 0
```

returns `true`.

Now let's look at powers of two that are **not** powers of four:

```text
2  = 00000010
8  = 00001000
32 = 00100000
```

Although each number still contains exactly one `1` bit, it falls into a position where the mask contains `0`. As a result:

```kotlin
(n and 0x55555555) != 0
```

returns `false`.

Putting both conditions together gives us the final solution:

```kotlin
fun isPowerOfFour(n: Int): Boolean {
    return n > 0 &&
        (n and (n - 1)) == 0 &&
        (n and 0x55555555) != 0
}
```

**Complexity**

**Time complexity**: `O(1)`

All operations are simple integer and bitwise operations, so the running time is constant.

**Space complexity**: `O(1)`

The algorithm uses only a constant amount of extra memory.
