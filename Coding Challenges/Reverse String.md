# Reverse String
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Two%20Pointers-blue)

You are given a string represented as a `CharArray`. Reverse the order of its characters by modifying the original array in-place without allocating another array, using only `O(1)` extra memory.

### Constraints

```kotlin
1 <= s.size <= 10^5
```

### Examples

**Example 1**:

```kotlin
Input: s = ['k', 'o', 't', 'l', 'i', 'n']
Output: ['n', 'i', 'l', 't', 'o', 'k']
```

Explanation:

The characters appear in reverse order in the modified array. Reading them from left to right produces `niltok` instead of `kotlin`.

**Example 2**:

```kotlin
Input: s = ['c', 'o', 'd', 'e', '!']
Output: ['!', 'e', 'd', 'o', 'c']
```

Explanation:

The characters are reversed directly in the original array. Since the array contains an odd number of elements, the middle character `d` remains at the same index.

## Solution

Kotlin provides the `reversed()` function, but it creates and returns a new collection. This requires `O(n)` additional memory and does not satisfy the requirement to modify the original array using only constant extra space. The `CharArray.reverse()` function works in-place, but using it would hide the algorithm that this challenge is intended to demonstrate, so we will not consider it a solution.

A recursive implementation is another possible way to reverse the array. However, every recursive call adds a new frame to the call stack, resulting in `O(n)` additional space, so this approach also does not satisfy the memory constraint.

The main idea is to work with two positions, initially pointing to the first and last characters of the array. We swap the characters at these positions and then move both positions toward the center, repeating the process until they meet or cross.

Although this approach uses the Two Pointers pattern, we do not need to maintain two independent indices. We can iterate using the index of the character on the left and calculate the corresponding position on the right:

```kotlin
val oppositeIndex = s.lastIndex - index
```

The number of required swaps is also known in advance because the size of the array is available. Each swap places two characters into their final positions, so only half of the array needs to be processed:

```kotlin
s.size / 2
```

For an array with an even number of characters, every element is included in a swap. For an array with an odd number of characters, the middle element remains unchanged because it is already in its correct position.

```kotlin
fun reverseString(s: CharArray): Unit {
    repeat(s.size / 2) { index ->
        val oppositeIndex = s.lastIndex - index
        val temporary = s[index]

        s[index] = s[oppositeIndex]
        s[oppositeIndex] = temporary
    }
}
```

**Complexity**

Time complexity: `O(n)`

The algorithm performs `s.size / 2` swaps. Since the number of operations grows linearly with the size of the input array, the time complexity is `O(n)`.

Space complexity: `O(1)`

The algorithm stores only the current index, the opposite index, and one temporary character. The amount of additional memory does not depend on the size of the input array.

## Step-by-step

### Example 1

```kotlin
s = ['k', 'o', 't', 'l', 'i', 'n']
```

The array contains `6` elements, so the algorithm performs `6 / 2 = 3` swaps. On each step, `oppositeIndex` is calculated as `s.lastIndex - index`.

|    Step | `index` | `oppositeIndex` | Characters swapped | Array after the swap             |
| ------: | ------: | --------------: | :----------------: | :------------------------------- |
| Initial |       - |               - |          -         | `['k', 'o', 't', 'l', 'i', 'n']` |
|       1 |     `0` |             `5` |      `k` ↔ `n`     | `['n', 'o', 't', 'l', 'i', 'k']` |
|       2 |     `1` |             `4` |      `o` ↔ `i`     | `['n', 'i', 't', 'l', 'o', 'k']` |
|       3 |     `2` |             `3` |      `t` ↔ `l`     | `['n', 'i', 'l', 't', 'o', 'k']` |

After three swaps, every character is in its final position. The resulting array represents the reversed string `niltok`.

### Example 2

```kotlin
s = ['c', 'o', 'd', 'e', '!']
```

The array contains `5` elements, so the algorithm performs `5 / 2 = 2` swaps. Integer division discards the remainder, leaving the middle character at index `2` unchanged.

|    Step | `index` | `oppositeIndex` | Characters swapped | Array after the swap        |
| ------: | ------: | --------------: | :----------------: | :-------------------------- |
| Initial |       - |               - |          -         | `['c', 'o', 'd', 'e', '!']` |
|       1 |     `0` |             `4` |      `c` ↔ `!`     | `['!', 'o', 'd', 'e', 'c']` |
|       2 |     `1` |             `3` |      `o` ↔ `e`     | `['!', 'e', 'd', 'o', 'c']` |

After two swaps, the characters on both sides of the center are in their final positions. The middle character `d` does not need to move, and the resulting array represents the reversed string `!edoc`.
