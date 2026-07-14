# Find the Difference
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)
![Pattern](https://img.shields.io/badge/Pattern-Bit%20Manipulation-blue)

## Description

You are given two strings, `s` and `t`, where `t` contains exactly the same characters as `s`, plus one additional character. The characters may appear in any order, so comparing the strings position by position is not possible.

The goal is to identify the extra character that was added to `t`.

### Constraints

```kotlin
0 <= s.length <= 1000
t.length == s.length + 1
s and t consist of lowercase English letters.
```

### Examples

**Example 1**:

```kotlin
Input:  s = "abcd", t = "abcde"
Output: 'e'
```

**Example 2**:

```kotlin
Input:  s = "listen", t = "silentx"
Output: 'x'
```

**Example 3**:

```kotlin
Input:  s = "aabc", t = "cabaa"
Output: 'a'
```

## Solution 1. Kotlin Standard Library

The first idea may be to convert both strings into sets and calculate the difference between them. Since `t` contains one additional character, it seems that this character should be the only element left in the resulting set.

```kotlin
fun findTheDifference(s: String, t: String): Char {
    return (t.toSet() - s.toSet()).first()
}
```

This solution works when the added character does not already exist in `s`.

For example:

```kotlin
s = "abcd"
t = "abcde"
```

After converting both strings into sets:

```text
s = ['a', 'b', 'c', 'd']
t = ['a', 'b', 'c', 'd', 'e']
```

The difference between them contains only `'e'`, so the method returns the correct result.

However, a `Set` stores only unique elements and does not preserve how many times each character appears. Because duplicate characters are allowed, this approach does not work for every valid input.

For example:

```kotlin
s = "aab"
t = "aaab"
```

Both strings produce the same set:

```text
s = ['a', 'b']
t = ['a', 'b']
```

The difference is empty, even though the additional character is `'a'`. Calling `first()` on the empty set will also throw an exception.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

To handle duplicate characters correctly, we need an operation that preserves their number of occurrences. Instead of storing unique characters, we can use their numeric codes and compare the total values of both strings.

## Solution 2. Character Code Sum

Instead of storing characters, we can use their numeric codes.

Every character has a unique numeric representation (`'a'` is `97`, `'b'` is `98`, and so on). Since `t` contains all characters from `s` plus exactly one extra character, the difference between the sums of their character codes is exactly the code of the added character.

For example:

```kotlin
s = "abcd"
t = "abcde"
```

The sums are:

```text
s = 97 + 98 + 99 + 100 = 394
t = 97 + 98 + 99 + 100 + 101 = 495
```

The difference is:

```text
495 - 394 = 101
```

And `101` is simply the character `'e'`.

Unlike the previous solution, this approach correctly handles duplicate characters because every occurrence contributes to the total sum.

```kotlin
fun findTheDifference(s: String, t: String): Char {
    val sourceSum = s.sumOf { it.code }
    val targetSum = t.sumOf { it.code }

    return (targetSum - sourceSum).toChar()
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

Although this solution is already optimal, we can achieve the same result using the XOR operator instead of calculating the difference between two sums.

## Solution 3. XOR Trick

This solution is based on one simple property of the XOR operator:

```text
a xor a = 0
0 xor b = b
a xor b = b xor a
```

Since `t` contains all characters from `s` plus one extra character. Since XOR is commutative, matching characters can always be grouped together. Every matching pair then cancels itself out, leaving only the added character.
For example:

```text
a xor b xor c xor d xor a xor b xor c xor d xor e
```

After canceling all matching pairs, only the added character remains:

```text
0 xor e = e
```

The order of the characters does not matter, so the strings can be shuffled in any way.

```kotlin
fun findTheDifference(s: String, t: String): Char {
    var result = 0

    for (c in s) {
        result = result xor c.code
    }

    for (c in t) {
        result = result xor c.code
    }

    return result.toChar()
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`
