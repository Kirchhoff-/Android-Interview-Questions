# Isomorphic Strings
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Hash%20Table-blue)

Given two strings `s` and `t`, determine whether they are **isomorphic**.

Two strings are considered isomorphic if every character in `s` can always be replaced with the same character in `t`.

Each character must have exactly one mapping. This means:
- One character cannot map to two different characters.
- Two different characters cannot map to the same character.

The order of characters must also remain unchanged.

### Constraints

```kotlin
1 <= s.length <= 5 * 10^4
t.length == s.length
s and t consist of valid ASCII characters
```

### Examples

**Example 1**:

```kotlin
Input: s = "moon", t = "loop"
Output: true
```

`m` always maps to `l`, `o` always maps to `o`, and `n` always maps to `p`. Every mapping is consistent.

**Example 2**:

```kotlin
Input: s = "ab", t = "cc"
Output: false
```

Both `a` and `b` would need to map to `c`, but different characters cannot share the same mapping.

**Example 3**:

```kotlin
Input: s = "code", t = "lamp"
Output: true
```

`c` maps to `l`, `o` maps to `a`, `d` maps to `m`, and `e` maps to `p`. Every character has exactly one mapping, and all mappings are consistent.

## Solution.

This problem should be solved using a `HashMap`.

The main idea is to build a mapping between characters from `s` and characters from `t`. If the same character from `s` appears again, it must always map to the same character from `t`.

Let's try to implement this:

```kotlin
fun isIsomorphic(s: String, t: String): Boolean {
    val sToT: MutableMap<Char, Char> = mutableMapOf()

    for (i in 0..<s.length) {
        val sourceChar: Char = s[i]
        val targetChar: Char = t[i]

        val mappedTarget: Char? = sToT[sourceChar]

        if (mappedTarget != null && mappedTarget != targetChar) return false

        sToT[sourceChar] = targetChar
    }

    return true
}
```

This works for cases where the same character from `s` tries to map to different characters from `t`.

For example:

```kotlin
s = "foo"
t = "bar"
```

Here `o` first maps to `a`, but later tries to map to `r`, so the strings are not isomorphic.

However, this solution still misses one important case:

```kotlin
s = "ab"
t = "cc"
```

With one map, we would store:

```text
a -> c
b -> c
```

From the `s -> t` direction everything looks valid, because both `a` and `b` have exactly one mapping. But this is still wrong, because two different characters from `s` map to the same character from `t`.

So we need to check whether the target character is already used:

```kotlin
fun isIsomorphic(s: String, t: String): Boolean {
    val sToT: MutableMap<Char, Char> = mutableMapOf()

    for (i in 0..<s.length) {
        val sourceChar: Char = s[i]
        val targetChar: Char = t[i]

        val mappedTarget: Char? = sToT[sourceChar]

        if (mappedTarget != null && mappedTarget != targetChar) return false

        if (mappedTarget == null && sToT.values.contains(targetChar)) return false

        sToT[sourceChar] = targetChar
    }

    return true
}
```

This version is logically correct, but it has a performance problem.

Checking a key in a `HashMap` is fast, but checking `sToT.values.contains(targetChar)` requires scanning all stored values. This operation is `O(n)`, and since we do it inside the loop, the total time complexity becomes `O(n²)`.

To keep the solution efficient, we should store the reverse mapping as well:

```text
s -> t
t -> s
```

This gives us fast lookup in both directions.

```kotlin
fun isIsomorphic(s: String, t: String): Boolean {
    val sToT: MutableMap<Char, Char> = mutableMapOf()
    val tToS: MutableMap<Char, Char> = mutableMapOf()

    for (i in 0..<s.length) {
        val sourceChar: Char = s[i]
        val targetChar: Char = t[i]

        val mappedTarget: Char? = sToT[sourceChar]
        val mappedSource: Char? = tToS[targetChar]

        if (mappedTarget != null && mappedTarget != targetChar) return false
        if (mappedSource != null && mappedSource != sourceChar) return false

        sToT[sourceChar] = targetChar
        tToS[targetChar] = sourceChar
    }

    return true
}
```

Now each lookup is done through a `HashMap`, so the algorithm stays linear.

**Complexity**

**Time Complexity:** `O(n)`. 

We iterate through both strings only once, and each `HashMap` lookup takes `O(1)` on average.

**Space Complexity:** `O(n)`. 

In the worst case, every character is unique, so both `HashMap`s together store up to `2n` mappings. Since constant factors are ignored in Big-O notation, the space complexity is `O(n)`.
