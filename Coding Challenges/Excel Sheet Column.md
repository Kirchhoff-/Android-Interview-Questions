# Excel Sheet Column
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)

Given a positive integer `columnNumber`, return the corresponding column title used in an Excel sheet.

Excel columns are labeled using uppercase English letters. The first 26 columns are represented by `A` through `Z`, after which the labels continue with multiple letters: `AA`, `AB`, `AC`, and so on.

### Constraints

```kotlin
1 <= columnNumber <= 2^31 - 1
```

### Examples

**Example 1**:

```kotlin
Input: columnNumber = 3
Output: "C"
```

**Example 2**:

```kotlin
Input: columnNumber = 54
Output: "BB"
```

**Example 3**:

```kotlin
Input: columnNumber = 1000
Output: "ALL"
```

## Solution
The main difficulty of this problem is that Excel column titles look like a base-26 numeral system, but they do not work exactly like a normal positional numeral system.

In a regular base-26 system, digits would have values from `0` to `25`. Excel uses letters from `A` to `Z`, which represent values from `1` to `26`. There is no separate symbol for zero.

However, Kotlin arithmetic operations naturally work with zero-based remainders: `% 26` produces values from `0` to `25`. If we use Excel's `1..26` system directly, the two ranges do not match and the conversion will not work correctly.

The problem appears when we try to use the remainder as a letter index. The `% 26` operation can return values from `0` to `25`, while Excel letters are numbered from `1` to `26`. For example, `26 % 26` returns `0`, but Excel column `26` is `Z`, not `A`.

Before applying any mathematical operations, we need both systems to use the same starting point. Excel represents letters using values from `1` to `26`, while our Kotlin representation uses a zero-based range from `0` to `25`. Subtracting `1` converts the Excel representation into the range used by our algorithm: `1 - 1 = 0`, `2 - 1 = 1`, and `26 - 1 = 25`.

After this conversion, we can use `% 26` to determine the current letter. The result is always a value from `0` to `25`, which directly corresponds to a letter from `A` to `Z`.

To split the whole number into letters, we need to know two things on each step: which letter should be added to the result and whether there is still another part of the number to process.

The remainder `% 26` gives us the current letter, while division by `26` gives us the remaining number. We continue this process until the remaining number becomes `0`.

The letters are found from right to left, so after processing the whole number, we need to reverse the result.

```kotlin
fun convertToTitle(columnNumber: Int): String {
    val letters = mapOf(
        0 to 'A',
        1 to 'B',
        2 to 'C',
        3 to 'D',
        4 to 'E',
        5 to 'F',
        6 to 'G',
        7 to 'H',
        8 to 'I',
        9 to 'J',
        10 to 'K',
        11 to 'L',
        12 to 'M',
        13 to 'N',
        14 to 'O',
        15 to 'P',
        16 to 'Q',
        17 to 'R',
        18 to 'S',
        19 to 'T',
        20 to 'U',
        21 to 'V',
        22 to 'W',
        23 to 'X',
        24 to 'Y',
        25 to 'Z',
    )

    var number = columnNumber
    val lettersNumber: MutableList<Int> = mutableListOf()

    while (number > 0) {
        val newLetterNumber: Int = (number - 1) % letters.size
        val hasMoreGroups: Int = (number - 1) / letters.size

        lettersNumber.add(newLetterNumber)

        number = hasMoreGroups
    }

    return buildString {
        lettersNumber.forEach { number ->
            append(letters[number])
        }
    }.reversed()
}
```

**Complexity**

- **Time complexity:** `O(log₂₆ n)` - on each iteration, the remaining number is divided by `26`. Building and reversing the result take the same number of steps as there are letters in the column title.

- **Space complexity:** `O(log₂₆ n)` - we store one number for each letter in the resulting column title. The `letters` map always contains exactly `26` elements and therefore uses constant space.

## Step-by-step

Let's walk through several examples.

### Example 1

```kotlin
columnNumber = 24
```

| Current number | Letter calculation | Current letter | Remaining number |
|---:|---|---:|---:|
| `24` | `(24 - 1) % 26 = 23` | `X` | `(24 - 1) / 26 = 0` |

The result is `X`.

### Example 2

```kotlin
columnNumber = 56
```

| Current number | Letter calculation | Current letter | Remaining number |
|---:|---|---:|---:|
| `56` | `(56 - 1) % 26 = 3` | `D` | `(56 - 1) / 26 = 2` |
| `2` | `(2 - 1) % 26 = 1` | `B` | `(2 - 1) / 26 = 0` |

The letters are collected from right to left: `D -> B`. After reversing, we get `BD`.

### Example 3

```kotlin
columnNumber = 702
```

| Current number | Letter calculation | Current letter | Remaining number |
|---:|---|---:|---:|
| `702` | `(702 - 1) % 26 = 25` | `Z` | `(702 - 1) / 26 = 26` |
| `26` | `(26 - 1) % 26 = 25` | `Z` | `(26 - 1) / 26 = 0` |

The result is `ZZ`.

### Example 4

```kotlin
columnNumber = 703
```

| Current number | Letter calculation | Current letter | Remaining number |
|---:|---|---:|---:|
| `703` | `(703 - 1) % 26 = 0` | `A` | `(703 - 1) / 26 = 27` |
| `27` | `(27 - 1) % 26 = 0` | `A` | `(27 - 1) / 26 = 1` |
| `1` | `(1 - 1) % 26 = 0` | `A` | `(1 - 1) / 26 = 0` |

The result is `AAA`.

This is an important boundary:

```text
702 -> ZZ
703 -> AAA
```

### Example 5

```kotlin
columnNumber = 1000
```

| Current number | Letter calculation | Current letter | Remaining number |
|---:|---|---:|---:|
| `1000` | `(1000 - 1) % 26 = 11` | `L` | `(1000 - 1) / 26 = 38` |
| `38` | `(38 - 1) % 26 = 11` | `L` | `(38 - 1) / 26 = 1` |
| `1` | `(1 - 1) % 26 = 0` | `A` | `(1 - 1) / 26 = 0` |

The letters are collected from right to left: `L -> L -> A`. After reversing, we get `ALL`.

### Example 6

```kotlin
columnNumber = 18279
```

| Current number | Letter calculation | Current letter | Remaining number |
|---:|---|---:|---:|
| `18279` | `(18279 - 1) % 26 = 0` | `A` | `(18279 - 1) / 26 = 703` |
| `703` | `(703 - 1) % 26 = 0` | `A` | `(703 - 1) / 26 = 27` |
| `27` | `(27 - 1) % 26 = 0` | `A` | `(27 - 1) / 26 = 1` |
| `1` | `(1 - 1) % 26 = 0` | `A` | `(1 - 1) / 26 = 0` |

The result is `AAAA`.
