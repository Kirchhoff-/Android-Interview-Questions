# Number of 1 Bits
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)
![Pattern](https://img.shields.io/badge/Pattern-Bit%20Manipulation-blue)

Given an integer `n`, return the number of `1` bits in its binary representation.

This value is also known as the **Hamming weight** of the number.

### Examples

**Input:** `11`  
**Output:** `3`  
**Explanation:** `11` in binary is `1011`, which contains three `1` bits.

**Input:** `64`  
**Output:** `1`  
**Explanation:** `64` in binary is `1000000`, which contains one `1` bit.

**Input:** `7`  
**Output:** `3`  
**Explanation:** `7` in binary is `111`, which contains three `1` bits.

## Solution 1. Standard Library API

Kotlin provides an API that allows us to convert an `Int` into its binary representation. Once converted, we can simply count how many characters are equal to `1`.

This is probably the most straightforward and intuitive solution to the problem.

```kotlin
fun hammingWeight(n: Int): Int {
    return n.toString(2).count { it == '1' }
}
```

**Complexity:**

* Time complexity: `O(n)`
* Space complexity: `O(n)`

There is also another standard library API that solves the same problem more efficiently:

```kotlin
fun hammingWeight(n: Int): Int {
    return Integer.bitCount(n)
}
```

**Complexity:**

* Time complexity: `O(1)`
* Space complexity: `O(1)`

While `Integer.bitCount()` would likely be the preferred solution in a real project, interview questions are usually designed to evaluate understanding of the underlying concepts rather than knowledge of existing APIs.

## Solution 2. Bit Manipulation

This is the solution most interviewers expect to see during a coding interview.

The key observation is that the following operation removes the rightmost set bit (`1`) from a number:

```kotlin
number = number and (number - 1)
```

For example:

```text
1101
1100
----
1100
```

Notice how the rightmost `1` bit disappears after the operation.

Let's see how this works step by step for the number `11`.

```text
11 -> 1011
```

**Iteration 1**

```text
1011
1010
----
1010
```

One set bit was removed.

**Iteration 2**

```text
1010
1001
----
1000
```

One more set bit was removed.

**Iteration 3**

```text
1000
0111
----
0000
```

The last set bit was removed. The loop executed **3 times**, which means the number contained **3 set bits**.

Using this observation, we can repeatedly remove set bits until the number becomes `0` and count how many iterations were required.

```kotlin
fun hammingWeight(n: Int): Int {
    var number = n
    var count = 0

    while (number != 0) {
        number = number and (number - 1)
        count++
    }

    return count
}
```

**Complexity:**

* Time complexity: `O(k)`
* Space complexity: `O(1)`

Where `k` is the number of set bits in the binary representation of the number.
