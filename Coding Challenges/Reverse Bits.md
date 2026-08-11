# Reverse Bits
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Bit%20Manipulation-blue)

Given a 32-bit signed integer `n`, reverse the order of its bits and return the resulting integer.

The binary representation must always be treated as exactly 32 bits, including any leading zeros. After reversing, the leftmost bit becomes the rightmost bit, the second bit becomes the second-to-last bit, and so on.

### Constraints

```kotlin
0 <= n <= 2^31 - 2
n is even
```

### Examples

**Example 1**:

```kotlin
Input: n = 10
Output: 1342177280
```

**Explanation**:

```text
00000000000000000000000000001010
01010000000000000000000000000000
```

**Example 2**:

```kotlin
Input: n = 12
Output: 805306368
```

**Explanation**:

```text
00000000000000000000000000001100
00110000000000000000000000000000
```

**Example 3**:

```kotlin
Input: n = 0
Output: 0
```

**Explanation**:

```text
00000000000000000000000000000000
00000000000000000000000000000000
```

## Solution 1. String Conversion

A straightforward way to solve this problem is to work with the binary representation of the number as a string. This is a good example of a solution we might prefer in regular application code: instead of manually manipulating individual bits, we can express the whole transformation using standard library functions, keeping the implementation short and easy to understand.

We can convert the number to its binary representation using `toString(2)`, reverse the resulting string, and convert it back using `toInt(2)`. There is only one important detail we need to handle: `toString(2)` does not preserve leading zeros.

For example:

```text
10 -> 1010
```

The problem, however, requires us to treat every number as a 32-bit value, so the actual representation we need to reverse is:

```text
00000000000000000000000000001010
```

To restore these missing zeros, we can use `padStart()`. This function is probably less familiar than common string operations such as `reversed()`, but it does exactly what we need here: it extends a string to the requested length by adding a specified character at the beginning.

For example:

```kotlin
"1010".padStart(8, '0')
```

produces:

```text
00001010
```

With that small detail handled, the solution becomes very simple: convert the number to binary, extend its representation to exactly 32 bits, reverse it, and convert the result back to an integer.

```kotlin
fun reverseBits(n: Int): Int {
    return n.toString(2)
        .padStart(32, '0')
        .reversed()
        .toInt(2)
}
```

**Complexity**

Time complexity: `O(1)`

Space complexity: `O(1)`

Both complexities are constant because we always work with a fixed 32-bit representation. The number of operations and the maximum size of the created strings do not depend on the value of `n`.

## Solution 2. Bit Manipulation

> **Disclaimer**
>
> While working on this problem, I found and reviewed around ten different bit manipulation solutions. Some of them may be more efficient than the approach below, but they also rely on considerably more complex combinations of masks, shifts, and bit-level tricks. At some point, the amount of "magic" involved makes the solution much harder to understand and explain.
>
> The solution below is the one I prefer because every operation has a clear purpose and the whole algorithm can be followed step by step.

The general idea is to take the rightmost bit from `number` and place it into the rightmost position of `result`. Then we shift `number` one position to the right, so the next bit becomes the rightmost one, and shift `result` one position to the left to make space for the next bit.

First, we need to extract the rightmost bit from `number`. We can do this using `and 1`: since `1` has only its rightmost bit set, all other bits are discarded and the result is always either `0` or `1`.

```kotlin
val bit = number and 1
```

Next, we need to add this bit to `result`. We shift `result` one position to the left using `shl 1`, which frees its rightmost position, and then use `or` to place `bit` there.

```kotlin
result = (result shl 1) or bit
```

Finally, we shift `number` one position to the right using `ushr 1`. The bit we have just processed is discarded, and the next bit becomes the rightmost one, ready for the next iteration.

```kotlin
number = number ushr 1
```

By doing this, we read `number` from right to left while building `result` from left to right. Since we are working with a 32-bit integer, we repeat these three operations exactly 32 times.

```kotlin
fun reverseBits(n: Int): Int {
    var number = n
    var result = 0

    repeat(32) {
        val bit = number and 1

        result = (result shl 1) or bit
        number = number ushr 1
    }

    return result
}
```

**Complexity**

Time complexity: `O(1)`

Space complexity: `O(1)`

The loop always performs exactly 32 iterations, regardless of the value of `n`, and the algorithm uses only a few integer variables.

## Step-by-step

Using the full 32-bit representation would make the examples unnecessarily long and difficult to follow. To make each operation easier to see, we will use 6-bit representations below. The original algorithm works in exactly the same way, but performs 32 iterations.

**Example 1**

```text
number = 110101
```

| Step | `number` | `bit` | `result` | Action |
|---:|:---:|:---:|:---:|---|
| 1 | `110101` | `1` | `000001` | Take the rightmost `1`, shift `result` left, and add the bit. |
| 2 | `011010` | `0` | `000010` | Take `0`, shift `result` left, and add the bit. |
| 3 | `001101` | `1` | `000101` | Take `1`, shift `result` left, and add the bit. |
| 4 | `000110` | `0` | `001010` | Take `0`, shift `result` left, and add the bit. |
| 5 | `000011` | `1` | `010101` | Take `1`, shift `result` left, and add the bit. |
| 6 | `000001` | `1` | `101011` | Take the final `1`, shift `result` left, and add the bit. |

After six iterations:

```text
110101 -> 101011
```

**Example 2**
```text
number = 001011
```

| Step | `number` | `bit` | `result` | Action |
|---:|:---:|:---:|:---:|---|
| 1 | `001011` | `1` | `000001` | Take the rightmost `1`, shift `result` left, and add the bit. |
| 2 | `000101` | `1` | `000011` | Take `1`, shift `result` left, and add the bit. |
| 3 | `000010` | `0` | `000110` | Take `0`, shift `result` left, and add the bit. |
| 4 | `000001` | `1` | `001101` | Take `1`, shift `result` left, and add the bit. |
| 5 | `000000` | `0` | `011010` | Take `0`, shift `result` left, and add the bit. |
| 6 | `000000` | `0` | `110100` | Take the final `0`, shift `result` left, and add the bit. |

After six iterations:

```text
001011 -> 110100
```
