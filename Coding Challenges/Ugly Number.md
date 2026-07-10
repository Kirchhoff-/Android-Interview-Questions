# Ugly Number
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)

An **ugly number** is a positive integer whose prime factors, if any, are limited to `2`, `3`, and `5`.

Given an integer `n`, determine whether it is an ugly number.

### Constraints

```kotlin
-2^31 <= n <= 2^31 - 1
```

### Examples

**Example 1**:

```kotlin
Input: n = 30
Output: true
```

Explanation:

```
30 = 2 × 3 × 5
```

All prime factors belong to the allowed set.

**Example 2**:

```kotlin
Input: n = 8
Output: true
```

Explanation:

```
8 = 2 × 2 × 2
```

A prime factor may appear multiple times and the number is still considered ugly.

**Example 3**:

```kotlin
Input: n = 42
Output: false
```

Explanation:

```
42 = 2 × 3 × 7
```

The prime factor `7` is not allowed.

**Example 4**:

```kotlin
Input: n = 1
Output: true
```

Explanation:

`1` has no prime factors. Since there are no prime factors outside `{2, 3, 5}`, it satisfies the definition of an ugly number.

**Example 5**:

```kotlin
Input: n = -25
Output: false
```

Explanation:

Only **positive** integers can be ugly numbers.

## Solution

The problem already tells us exactly which prime factors are allowed: **2**, **3**, and **5**.

Instead of trying to determine whether a number is ugly directly, we can simply remove every occurrence of these prime factors. We repeatedly divide the number by `2` while it is divisible by `2`, then do the same for `3`, and finally for `5`.

If, after removing all allowed prime factors, the remaining value is `1`, then the original number contained no other prime factors and is an ugly number. Otherwise, at least one disallowed prime factor must have been present.

```kotlin
fun isUgly(n: Int): Boolean {
    if (n <= 0) return false

    var number: Int = n

    while (number % 2 == 0) {
        number /= 2
    }

    while (number % 3 == 0) {
        number /= 3
    }

    while (number % 5 == 0) {
        number /= 5
    }

    return number == 1
}
```

**Complexity**

**Time complexity**: `O(log n)`

Each successful division reduces the value of `number`, so the total number of iterations is logarithmic with respect to the input value.

**Space complexity**: `O(1)`

The algorithm uses only a single variable regardless of the input size.

### Step-by-step

Let's walk through several examples.

#### Example 1: `n = 8`

```text
8 / 2 = 4
4 / 2 = 2
2 / 2 = 1
```

The remaining value is `1`, so `8` is an ugly number.

---

#### Example 2: `n = 90`

```text
90 / 2 = 45
45 / 3 = 15
15 / 3 = 5
5 / 5 = 1
```

After removing all factors `2`, `3`, and `5`, the remaining value is `1`. Therefore, `90` is an ugly number.

---

#### Example 3: `n = 70`

```text
70 / 2 = 35
35 / 5 = 7
```

The remaining value is `7`, which means the original number contains a prime factor other than `2`, `3`, or `5`.

Therefore, `70` is not an ugly number.
