# Excel Sheet Column Number
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)

Given a string `columnTitle` representing a column title in an Excel sheet, return its corresponding column number. Columns are labeled from `A` to `Z`, representing numbers `1` to `26`. After `Z`, the titles continue with `AA`, `AB`, and so on.

## Constraints

* `1 <= columnTitle.length <= 7`
* `columnTitle` contains only uppercase English letters.
* `columnTitle` is in the range `["A", "FXSHRXW"]`.

## Examples

**Example 1:**

```text
Input: columnTitle = "A"
Output: 1
```

**Example 2:**

```text
Input: columnTitle = "AB"
Output: 28
```

**Example 3:**

```text
Input: columnTitle = "ABC"
Output: 731
```

## Solution

This problem is the reverse of [Excel Sheet Column](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Excel%20Sheet%20Column.md). There, we convert a column number into a title; here, we convert the title back into a number.

Each letter has a value from `1` to `26`. We read the title from left to right, multiplying the current number by `26` before adding the value of the next letter. For example, `AB` gives us `1 × 26 + 2 = 28`.

```kotlin
fun titleToNumber(columnTitle: String): Int {
    val letters = mapOf(
        'A' to 1,
        'B' to 2,
        'C' to 3,
        'D' to 4,
        'E' to 5,
        'F' to 6,
        'G' to 7,
        'H' to 8,
        'I' to 9,
        'J' to 10,
        'K' to 11,
        'L' to 12,
        'M' to 13,
        'N' to 14,
        'O' to 15,
        'P' to 16,
        'Q' to 17,
        'R' to 18,
        'S' to 19,
        'T' to 20,
        'U' to 21,
        'V' to 22,
        'W' to 23,
        'X' to 24,
        'Y' to 25,
        'Z' to 26,
    )

    var number = 0

    for (letter in columnTitle) {
        number *= letters.size
        number += letters.getValue(letter)
    }

    return number
}
```

**Complexity**

* **Time complexity:** `O(n)`, where `n` is the length of `columnTitle`. We process each letter once; creating the fixed-size map takes constant time.
* **Space complexity:** `O(1)`. The map always contains `26` entries, and the calculation uses one additional integer.

## Step-by-step

### Example 1: `ABC`

| Letter | Letter value |   Calculation | Updated `number` |
| ------ | -----------: | ------------: | ---------------: |
| `A`    |          `1` |  `0 × 26 + 1` |              `1` |
| `B`    |          `2` |  `1 × 26 + 2` |             `28` |
| `C`    |          `3` | `28 × 26 + 3` |            `731` |

### Example 2: `AZ`

| Letter | Letter value |   Calculation | Updated `number` |
| ------ | -----------: | ------------: | ---------------: |
| `A`    |          `1` |  `0 × 26 + 1` |              `1` |
| `Z`    |         `26` | `1 × 26 + 26` |             `52` |

