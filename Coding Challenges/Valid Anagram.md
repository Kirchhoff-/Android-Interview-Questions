# Valid Anagram

![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-HashMap-blue)

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`. Otherwise, return `false`. An anagram is a word or phrase formed by rearranging the characters of another word or phrase while using every original character exactly once.

### Constraints

```kotlin
1 <= s.length, t.length <= 5 * 10^4
`s` and `t` consist of lowercase English letters.
```

### Examples

**Example 1**:

```kotlin
Input: s = "listen", t = "silent"
Output: true
```

Explanation:

Both strings contain exactly the same characters with the same frequencies. The characters appear in a different order, so `t` is an anagram of `s`.

**Example 2**:

```kotlin
Input: s = "a", t = "aa"
Output: false
```

Explanation:

The string `s` contains one `a`, while `t` contains two. Their character frequencies are different, so `t` is not an anagram of `s`.

**Example 3**:

```kotlin
Input: s = "aacc", t = "ccac"
Output: false
```

Explanation:

Both strings have the same length and contain the same unique characters. However, `s` contains two `a` and two `c` characters, while `t` contains one `a` and three `c` characters, so `t` is not an anagram of `s`.

## Solution

First, compare the lengths of the strings. Strings of different lengths cannot be anagrams, so in this case we can immediately return `false` without building the frequency map.

Next, create a mutable map containing the frequency of each character in `s`. While iterating through `t`, decrement the corresponding frequency and remove the character from the map when its frequency reaches zero.

If a character from `t` is missing from the map, `t` contains a character or more occurrences of a character than `s`, so the function returns `false`. After processing all characters, the map must be empty, meaning that every character from `s` was matched.

```kotlin
fun isAnagram(s: String, t: String): Boolean {
    if (s.length != t.length) return false

    val sMap = s.groupingBy { it }.eachCount().toMutableMap()

    for (entry in t) {
        sMap[entry]?.let { value ->
            val newValue = value - 1

            if (newValue == 0) {
                sMap.remove(entry)
            } else {
                sMap[entry] = newValue
            }
        } ?: return false
    }

    return sMap.isEmpty()
}
```

**Complexity**

**Time complexity**: `O(n)`, where `n` is the length of each string.

**Space complexity**: `O(k)`, where `k` is the number of unique characters in `s`. Since the input contains only lowercase English letters, `k` cannot exceed `26`, so the space complexity can also be considered `O(1)` for the given constraints.

## Step-by-step

For `s = "listen"` and `t = "silent"`, both strings have a length of `6`, so the algorithm continues. The frequency map created from `s` initially contains one occurrence of each character.

|    Step | Character from `t` | Action                            | Frequency map                    |
| ------: | :----------------: | --------------------------------- | -------------------------------- |
| Initial |          —         | Count the characters in `s`       | `{l=1, i=1, s=1, t=1, e=1, n=1}` |
|       1 |         `s`        | Frequency becomes `0`; remove `s` | `{l=1, i=1, t=1, e=1, n=1}`      |
|       2 |         `i`        | Frequency becomes `0`; remove `i` | `{l=1, t=1, e=1, n=1}`           |
|       3 |         `l`        | Frequency becomes `0`; remove `l` | `{t=1, e=1, n=1}`                |
|       4 |         `e`        | Frequency becomes `0`; remove `e` | `{t=1, n=1}`                     |
|       5 |         `n`        | Frequency becomes `0`; remove `n` | `{t=1}`                          |
|       6 |         `t`        | Frequency becomes `0`; remove `t` | `{}`                             |

The frequency map is empty after every character from `t` has been processed. Therefore, `"silent"` is an anagram of `"listen"`, and the function returns `true`.
