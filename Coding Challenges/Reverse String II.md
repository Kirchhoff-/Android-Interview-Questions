# Reverse String II
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Two%20Pointers-blue)

Given a string `s` and a positive integer `k`, divide the string into consecutive groups containing up to `k` characters. Reverse the groups in the first, third, fifth, and subsequent odd-numbered positions, while preserving the other groups. Continue this pattern until the end of the string and return the resulting string.

The final group may contain fewer than `k` characters. If that group is selected for reversal, reverse all the characters it contains; otherwise, preserve their original order.

### Constraints

```text
1 <= s.length <= 10^4
s contains only lowercase English letters
1 <= k <= 10^4
```

### Examples

**Example 1**:

```kotlin
Input: s = "abcdefghij", k = 2
Output: "bacdfeghji"
```

Explanation:

Divide the string into groups of two characters. Reverse the groups in the first, third, and fifth positions, while preserving the second and fourth groups:

```text
ab | cd | ef | gh | ij
ba | cd | fe | gh | ji
```

**Example 2**:

```kotlin
Input: s = "abcdefghij", k = 4
Output: "dcbaefghji"
```

Explanation:

The first two groups contain four characters, while the final group contains only two. Since the final group is in the third position, all of its available characters are reversed:

```text
abcd | efgh | ij
dcba | efgh | ji
```

**Example 3**:

```kotlin
Input: s = "abcdef", k = 4
Output: "dcbaef"
```

Explanation:

Only the first group is selected for reversal. The remaining group is in the second position, so its characters preserve their original order:

```text
abcd | ef
dcba | ef
```

## Solution 1. Kotlin API

The simplest way to solve this problem in Kotlin is to divide the string into groups of `k` characters. The Kotlin standard library already provides the [`chunked()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.text/chunked.html) function, which performs exactly this operation and also handles the final group when it contains fewer than `k` characters.

Once the groups are created, we can process them using `forEachIndexed()`. Groups with indices `0`, `2`, `4`, and so on correspond to the first, third, fifth, and subsequent odd-numbered positions, so we reverse them and append the remaining groups without changes.

This solution is intentionally simple and closely follows the problem description. The only Kotlin-specific detail required to understand it is the `chunked()` function, while the rest of the code directly expresses which groups should be reversed.

This would most likely be the preferred implementation in production code. 

```kotlin
fun reverseString(s: String, k: Int): String {
    val resultString = StringBuilder()

    s.chunked(k).forEachIndexed { index, chunk ->
        if (index % 2 == 0) {
            resultString.append(chunk.reversed())
        } else {
            resultString.append(chunk)
        }
    }

    return resultString.toString()
}
```

**Complexity**

Time complexity: `O(n)`

The `chunked()` function traverses the entire string to divide it into groups, after which every character is appended to the result. Reversing the selected groups also processes each of their characters, so the total amount of work grows linearly with the length of the string.

Space complexity: `O(n)`

The list created by `chunked()`, the reversed groups, and the `StringBuilder` all require additional memory proportional to the length of the input string. The resulting string also contains the same number of characters as the original string.

## Solution 2. Two Pointers

During an interview, this is most likely the implementation an interviewer would expect. It demonstrates that we can work directly with indices, reverse parts of a string manually, and handle array boundaries without relying on higher-level Kotlin functions.

Since `String` is immutable in Kotlin, we first convert it to a `CharArray`. This gives us a mutable collection in which characters can be swapped without creating a new string after every operation.

We process the array in consecutive groups of `k` characters. The first group must be reversed, the second group must remain unchanged, and this pattern continues until the end of the array.

To represent this alternation, we use a boolean flag. It initially indicates that the current group should be reversed and is inverted after every group, ensuring that the next operation is always the opposite of the previous one.

When a group must be reversed, we place one pointer at its first character and another pointer at its last character. The left pointer already represents the beginning of the group, while the position of the right pointer must be calculated.

A complete group contains `k` characters, so its last character is located at:

```kotlin
leftIndex + k - 1
```

The subtraction is necessary because the character at `leftIndex` is already included in the group. For example, a group of three characters starting at index `4` occupies indices `4`, `5`, and `6`, so its right boundary is `4 + 3 - 1 = 6`.

The final group may contain fewer than `k` characters, which means that the calculated boundary could be outside the array. To keep the right pointer within the valid range, we choose the smaller value between the expected end of the group and the last available index:

```kotlin
minOf(leftIndex + k - 1, characters.lastIndex)
```

After defining both boundaries, we swap the characters and move the pointers toward the center. This continues until the pointers meet and the selected group is completely reversed.

```kotlin
fun reverseString(s: String, k: Int): String {
    val characters = s.toCharArray()
    var shouldReverse = true

    for (groupStartIndex in characters.indices step k) {
        if (shouldReverse) {
            var leftIndex = groupStartIndex
            var rightIndex = minOf(leftIndex + k - 1, characters.lastIndex)

            while (leftIndex < rightIndex) {
                val character = characters[leftIndex]
                characters[leftIndex] = characters[rightIndex]
                characters[rightIndex] = character

                leftIndex++
                rightIndex--
            }
        }

        shouldReverse = !shouldReverse
    }

    return characters.concatToString()
}
```

**Complexity**

Time complexity: `O(n)`

Converting the string to a `CharArray` and creating the resulting string both require linear time. Reversing the selected groups also processes each affected character only once, so the total time complexity remains linear.

Space complexity: `O(n)`

The algorithm creates a `CharArray` containing all characters from the original string. Its size grows linearly with the input, while the two pointers and the boolean variable require only constant additional space.

## Step-by-step

The boolean flag starts as `true` because the first group must be reversed. After each group is processed, the value changes so that the next group receives the opposite operation.

### Example 1

```kotlin
s = "abcdefghij"
k = 2
```

The string is processed in five groups containing two characters each. The groups in the first, third, and fifth positions are reversed, while the remaining groups preserve their original order.

| Group position | Indices | Characters | `shouldReverse` | Processed group | Current array |
| -------------- | ------- | ---------- | --------------- | --------------- | ------------- |
| 1              | `0..1`  | `ab`       | `true`          | `ba`            | `ba`          |
| 2              | `2..3`  | `cd`       | `false`         | `cd`            | `bacd`        |
| 3              | `4..5`  | `ef`       | `true`          | `fe`            | `bacdfe`      |
| 4              | `6..7`  | `gh`       | `false`         | `gh`            | `bacdfegh`    |
| 5              | `8..9`  | `ij`       | `true`          | `ji`            | `bacdfeghji`  |

### Example 2

```kotlin
s = "abcdefghij"
k = 4
```

The first two groups contain four characters, while the final group contains only two. The final group is in the third position, so all of its available characters are reversed.

| Group position | Indices | Characters | `shouldReverse` | Processed group | Current array |
| -------------- | ------- | ---------- | --------------- | --------------- | ------------- |
| 1              | `0..3`  | `abcd`     | `true`          | `dcba`          | `dcba`        |
| 2              | `4..7`  | `efgh`     | `false`         | `efgh`          | `dcbaefgh`    |
| 3              | `8..9`  | `ij`       | `true`          | `ji`            | `dcbaefghji`  |

### Example 3

```kotlin
s = "abcdef"
k = 4
```

The first group contains four characters and is reversed. The final group is in the second position, so it remains unchanged even though it contains fewer than `k` characters.

| Group position | Indices | Characters | `shouldReverse` | Processed group | Current array |
| -------------- | ------- | ---------- | --------------- | --------------- | ------------- |
| 1              | `0..3`  | `abcd`     | `true`          | `dcba`          | `dcba`        |
| 2              | `4..5`  | `ef`       | `false`         | `ef`            | `dcbaef`      |
