# First Unique Character in a String
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Hash%20Table-blue)

Given a string `s`, return the index of the **first unique** character.

A character is considered unique if it appears **exactly once** in the string. If no such character exists, return `-1`.

## Constraints

```kotlin
1 <= s.length <= 10^5
```
`s` consists of only lowercase English letters

## Examples

**Example 1**:

```kotlin
Input: s = "aabcdd"
Output: 2
```

The character `'b'` appears exactly once and is the first unique character in the string.

---

**Example 2**:

```kotlin
Input: s = "aabbccd"
Output: 6
```

All characters except `'d'` appear more than once, so its index is returned.

---

**Example 3**:

```kotlin
Input: s = "xxyyzz"
Output: -1
```

Every character appears more than once, so there is no unique character.

## Solution

The first thing we need to know is how many times each character appears in the string. A character can be considered unique only after we know that its total count is equal to `1`.

After building this frequency information, we traverse the string from left to right and return the first index whose character appears only once.

We cannot return the answer during the first traversal. Even if a character has appeared only once so far, the same character may still appear later in the string. Because of this, we first count how many times each character occurs, and only then perform a second pass to find the first character whose frequency is equal to 1.

```kotlin
fun firstUniqChar(s: String): Int {
    val charCountMap = mutableMapOf<Char, Int>()

    for (char in s) {
        val count = charCountMap[char]

        if (count == null) {
            charCountMap[char] = 1
        } else {
            charCountMap[char] = count + 1
        }
    }

    for (index in s.indices) {
        if (charCountMap[s[index]] == 1) {
            return index
        }
    }

    return -1
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

As an alternative to the second pass over the string, we can store not only the character count, but also the first index where this character appeared. This means that after processing the string once, we can inspect the stored character information and return the unique character with the smallest first index.

Although this approach avoids a second traversal of the original string, it still performs additional operations such as filtering and finding the minimum index.  Conceptually, this is still another pass over a collection. The only difference is that we iterate over the stored character information rather than the original string.

Since this collection can contain at most 26 elements, this additional processing takes constant time and the overall complexity remains `O(n)`.

```kotlin
data class CharInfo(
    var count: Int,
    val firstIndex: Int
)

fun firstUniqChar(s: String): Int {
    val charInfoMap: MutableMap<Char, CharInfo> = mutableMapOf()

    for (index in s.indices) {
        val char = s[index]
        val info = charInfoMap[char]

        if (info == null) {
            charInfoMap[char] = CharInfo(
                count = 1,
                firstIndex = index
            )
        } else {
            info.count++
        }
    }

    return charInfoMap
        .filter { it.value.count == 1 }
        .minByOrNull { it.value.firstIndex }
        ?.value
        ?.firstIndex
        ?: -1
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`
