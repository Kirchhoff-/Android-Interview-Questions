# Length of Last Word
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-String-blue)

## Description

Given a string `s`, return the length of its last word.

A word is defined as a sequence of non-space characters. The string may contain leading, trailing, or multiple spaces between words, but only the characters that belong to the last word should be counted.

### Examples

**Input:** `s = "Hello World"`  
**Output:** `5`  
**Explanation:** The last word is `"World"`.

**Input:** `s = "   fly me   to   the moon   "`  
**Output:** `4`  
**Explanation:** The last word is `"moon"`. Trailing spaces should not be counted.

**Input:** `s = "joyboy"`  
**Output:** `6`  
**Explanation:** The string contains only one word, so its length is the answer.

## Solution 1. Standard String APIs

A straightforward approach is to use Kotlin's built-in string functions.

First, we remove any trailing spaces using `trimEnd()`. Then we find the position of the last space character using `indexOfLast()`. Everything after that position belongs to the last word, so we can extract it with `substring()` and return its length.

```kotlin
fun lengthOfLastWord(s: String): Int {
    val stringWithoutEndSpaces: String = s.trimEnd()
    val indexOfLastSpace: Int = stringWithoutEndSpaces.indexOfLast { it == ' ' }

    return stringWithoutEndSpaces.substring(indexOfLastSpace + 1).length
}
```

Another implementation is:

```kotlin
fun lengthOfLastWord(s: String): Int {
    return s.trim().split(" ").last().length
}
```

It solves the problem using standard Kotlin APIs and follows the same general idea as the previous solution.

**Complexity:**

* Time complexity: `O(n)`
* Space complexity: `O(n)`

## Solution 2. Iterate From the End

Instead of creating additional strings, we can process the input directly.

Starting from the end of the string, we first locate the last non-space character. This gives us the end of the last word. We then continue moving left until we find a space character. The distance between these positions represents the length of the last word.

```kotlin
fun lengthOfLastWord(s: String): Int {
    var indexOfLastNonSpace: Int = -1

    for (i in s.length - 1 downTo 0) {
        when {
            s[i] == ' ' && indexOfLastNonSpace == -1 -> continue
            s[i] != ' ' && indexOfLastNonSpace == -1 -> indexOfLastNonSpace = i
            s[i] == ' ' && indexOfLastNonSpace != -1 -> return indexOfLastNonSpace - i
        }
    }

    return s.length
}
```

We can simplify this idea even further. Instead of storing the index of the last character and calculating the distance later, we can skip trailing spaces and count the characters of the last word directly.

```kotlin
fun lengthOfLastWord(s: String): Int {
    var index = s.length - 1

    while (s[index] == ' ') {
        index--
    }

    var length = 0

    while (index >= 0 && s[index] != ' ') {
        length++
        index--
    }

    return length
}
```

This approach produces the same result while using less state. Whether this version is more readable than the previous one is largely a matter of personal preference.

**Complexity:**

* Time complexity: `O(n)`
* Space complexity: `O(1)`
