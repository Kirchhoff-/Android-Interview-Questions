# Number Complement
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Bit%20Manipulation-blue)

You are given a positive integer `num`. Your task is to find its complement.

The complement is created by inverting every bit in the binary representation of the number. Leading zero bits are not included in the binary representation and therefore should not be inverted.

## Constraints

```kotlin
1 <= num < 2^31
```

### Examples

**Example 1**:

```kotlin
Input: num = 10
Output: 5
```

Explanation:

```text
10 = 1010
     0101 = 5
```

**Example 2**:

```kotlin
Input: num = 8
Output: 7
```

Explanation:

```text
8 = 1000
    0111 = 7
```

**Example 3**:

```kotlin
Input: num = 1
Output: 0
```

Explanation:

```text
1 = 1
    0 = 0
```

## Solution 1 - Kotlin API

The most straightforward way to solve this problem is to work directly with the binary representation of the number. Kotlin allows us to convert an integer to a binary `String` by passing `2` as the radix to `toString()`.

There are several ways to invert the characters in the resulting string. For example, we could iterate over them and replace each `0` with `1` and each `1` with `0`. In this solution, we will use a simple chain of `replace()` calls instead.

Since replacing `0` with `1` directly would make it impossible to distinguish the original `1`s afterward, we first replace every `0` with a temporary `2`. Then we replace the original `1`s with `0`s and finally replace the temporary `2`s with `1`s.

The resulting binary string can be converted back to an integer using `toInt(2)`.

```kotlin
fun findComplement(num: Int): Int {
    return num.toString(2)
        .replace("0", "2")
        .replace("1", "0")
        .replace("2", "1")
        .toInt(2)
}
```

**Complexity**

Time complexity: `O(log n)`

The binary representation of `num` contains `O(log n)` bits. Converting the number to a string, replacing its characters, and converting it back all require processing this representation.

Space complexity: `O(log n)`

The solution creates strings whose size depends on the number of bits in `num`.

## Solution 2 - XOR

We can avoid converting the number to a string and solve the problem using the `XOR` operation.

`XOR` with `1` inverts a bit:

```text
0 xor 1 = 1
1 xor 1 = 0
```

This means that if we create a mask consisting only of `1`s and apply it to `num`, all bits covered by the mask will be inverted.

For example:

```text
num  = 101
mask = 111

  101
xor
  111
-----
  010
```

So the main problem is to build a mask of the correct size. It should contain exactly as many `1`s as there are significant bits in `num`.

To determine this size, we make a copy of `num` and repeatedly shift it one bit to the right. Each shift removes one significant bit, so the number of iterations tells us how many bits our mask should contain.

At the same time, we build the mask. On every iteration, we shift the current mask to the left and append `1`:

```kotlin
mask = (mask shl 1) or 1
```

For `num = 5`:

```text
number: 101 -> 10 -> 1 -> 0
mask:     0 ->  1 -> 11 -> 111
```

When `number` reaches `0`, the mask is ready. We can then apply `XOR` to the original number and return its complement.

```kotlin
fun findComplement(num: Int): Int {
    var number = num
    var mask = 0

    while (number != 0) {
        mask = (mask shl 1) or 1
        number = number ushr 1
    }

    return num xor mask
}
```

**Complexity**

Time complexity: `O(log n)`

The loop runs once for every significant bit in `num`.

Space complexity: `O(1)`

The solution uses only a constant amount of additional memory.

## Step-by-step

### Example 1

For `num = 5`:

```text
5 = 101
```

| Step   | `number` | `mask` | Operation                               |
| ------ | -------- | ------ | --------------------------------------- |
| Start  | `101`    | `0`    | Initial values                          |
| 1      | `10`     | `1`    | Shift `number` right, add `1` to `mask` |
| 2      | `1`      | `11`   | Shift `number` right, add `1` to `mask` |
| 3      | `0`      | `111`  | Shift `number` right, add `1` to `mask` |
| Result |          | `010`  | `101 xor 111 = 010`                     |

`010` is `2` in decimal.

### Example 2

For `num = 10`:

```text
10 = 1010
```

| Step   | `number` | `mask` | Operation                               |
| ------ | -------- | ------ | --------------------------------------- |
| Start  | `1010`   | `0`    | Initial values                          |
| 1      | `101`    | `1`    | Shift `number` right, add `1` to `mask` |
| 2      | `10`     | `11`   | Shift `number` right, add `1` to `mask` |
| 3      | `1`      | `111`  | Shift `number` right, add `1` to `mask` |
| 4      | `0`      | `1111` | Shift `number` right, add `1` to `mask` |
| Result |          | `0101` | `1010 xor 1111 = 0101`                  |

`0101` is `5` in decimal.
