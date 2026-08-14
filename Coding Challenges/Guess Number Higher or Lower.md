# Guess Number Higher or Lower
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Binary%20Search-blue)

A number is chosen somewhere in the range from `1` to `n`. Your goal is to find it.

To do this, you are given the API `guess(number)`, which checks your candidate and returns one of three possible results:

* `-1` - the chosen number is lower than your candidate;
* `1` - the chosen number is higher than your candidate;
* `0` - your candidate is the chosen number.

Find and return the chosen number.

## Constraints

```kotlin
1 <= n <= 2^31 - 1
1 <= pick <= n
```

## Examples

**Example 1**:

```kotlin
Input: n = 15, pick = 11
Output: 11
```

Explanation:

The chosen number is `11`.

**Example 2**:

```kotlin
Input: n = 7, pick = 3
Output: 3
```

Explanation:

The chosen number is `3`.

**Example 3**:

```kotlin
Input: n = 1, pick = 1
Output: 1
```

Explanation:

The only possible number is `1`.

## Solution

> **Disclaimer:** This problem is almost identical to the [First Bad Version](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/First%20Bad%20Version.md) challenge. Both problems use Binary Search over a range of numbers and rely on an external API to decide which part of the range should be discarded.
>
> The only meaningful difference is how the API response affects the current candidate. In [First Bad Version](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/First%20Bad%20Version.md), a bad candidate may still be the answer, because it can be the first bad version. In this problem, if `guess()` returns 1 or -1, we know for sure that the current candidate is not the chosen number and can exclude it from the search range.

This is a classic Binary Search problem.

We start with the entire range from `1` to `n` and check the number in the middle. Based on the result returned by `guess()`, we discard the half where the chosen number cannot be and repeat the same process with the remaining range.

To calculate the middle, we use:

```kotlin
rangeStart + (rangeEnd - rangeStart) / 2
```

instead of:

```kotlin
(rangeStart + rangeEnd) / 2
```

Both formulas produce the same result, but the second one may cause an integer overflow when `rangeStart` and `rangeEnd` are large.

There is one more important detail when updating the range. If `guess()` returns `1` or `-1`, we already know that the current candidate is not the chosen number. Therefore, we can exclude it from the next search range:

```kotlin
rangeStart = newCandidateNumber + 1
```

or:

```kotlin
rangeEnd = newCandidateNumber - 1
```

This guarantees that every unsuccessful candidate is removed from the search range and that the range becomes smaller after every iteration.

```kotlin
fun guessNumber(n: Int): Int {
    var rangeStart = 1
    var rangeEnd = n

    while (true) {
        val newCandidateNumber = rangeStart + (rangeEnd - rangeStart) / 2

        val number = guess(newCandidateNumber)

        when (number) {
            0 -> return newCandidateNumber
            1 -> rangeStart = newCandidateNumber + 1
            -1 -> rangeEnd = newCandidateNumber - 1
        }
    }
}
```

**Complexity**

* Time complexity: `O(log n)` because after each call to `guess()` we discard approximately half of the remaining search range.

* Space complexity: `O(1)` because we only use a few variables to store the current search boundaries and candidate number.

## Step-by-Step

### Example 1

```kotlin
n = 15
pick = 11
```

| Step | Search range |                 Candidate | `guess()` | Result                                                   |
| ---- | ------------ | ------------------------: | --------: | -------------------------------------------------------- |
| 1    | `[1, 15]`    |    `1 + (15 - 1) / 2 = 8` |       `1` | `11` is higher than `8`, so the new range is `[9, 15]`   |
| 2    | `[9, 15]`    |   `9 + (15 - 9) / 2 = 12` |      `-1` | `11` is lower than `12`, so the new range is `[9, 11]`   |
| 3    | `[9, 11]`    |   `9 + (11 - 9) / 2 = 10` |       `1` | `11` is higher than `10`, so the new range is `[11, 11]` |
| 4    | `[11, 11]`   | `11 + (11 - 11) / 2 = 11` |       `0` | The chosen number is `11`                                |

### Example 2

```kotlin
n = 20
pick = 4
```

| Step | Search range |               Candidate | `guess()` | Result                                               |
| ---- | ------------ | ----------------------: | --------: | ---------------------------------------------------- |
| 1    | `[1, 20]`    | `1 + (20 - 1) / 2 = 10` |      `-1` | `4` is lower than `10`, so the new range is `[1, 9]` |
| 2    | `[1, 9]`     |   `1 + (9 - 1) / 2 = 5` |      `-1` | `4` is lower than `5`, so the new range is `[1, 4]`  |
| 3    | `[1, 4]`     |   `1 + (4 - 1) / 2 = 2` |       `1` | `4` is higher than `2`, so the new range is `[3, 4]` |
| 4    | `[3, 4]`     |   `3 + (4 - 3) / 2 = 3` |       `1` | `4` is higher than `3`, so the new range is `[4, 4]` |
| 5    | `[4, 4]`     |   `4 + (4 - 4) / 2 = 4` |       `0` | The chosen number is `4`                             |
