# Majority Element
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Array-blue)
![Pattern](https://img.shields.io/badge/Pattern-Hash%20Map-blue)

Given an integer array `nums`, return the majority element.

The majority element is the value that appears more than n / 2 times in the array. You may assume that the majority element always exists.

### Constraints

```kotlin
nums.size == n
1 <= n <= 5 * 10^4
-10^9 <= nums[i] <= 10^9
```

### Examples

**Example 1**:

```kotlin
Input: nums = [8, 3, 8]
Output: 8
```

**Example 2**:

```kotlin
Input: nums = [5, 1, 5, 2, 5, 5, 4]
Output: 5
```

**Example 3**:

```kotlin
Input: nums = [9]
Output: 9
```

## Solution 1. HashMap

The most straightforward way to solve this problem is to count how many times each value appears in the array.

We can use a `HashMap`, where the key is the number from the array and the value is the number of its occurrences. Once some number appears more than `n / 2` times, we can immediately return it.

```kotlin
fun majorityElement(nums: IntArray): Int {
    val frequencyMap: MutableMap<Int, Int> = mutableMapOf()
    val majorityCount: Int = (nums.size / 2) + 1

    for (num in nums) {
        if (frequencyMap.contains(num)) {
            val occurrences = frequencyMap.getValue(num) + 1
            frequencyMap[num] = occurrences

            if (occurrences >= majorityCount) return num
        } else {
            frequencyMap[num] = 1
        }
    }

    return 0
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

A similar solution can also be implemented using Kotlin standard library functions. Instead of manually maintaining a `HashMap`, `groupBy` groups identical numbers together, while `maxBy` returns the group containing the largest number of elements.

```kotlin
fun majorityElement(nums: IntArray): Int =
    nums
        .groupBy { it }
        .maxBy { it.value.size }
        .key
```

Although this solution is shorter, it follows the same counting-based approach as the `HashMap` solution above. The main difference is that `groupBy` creates a list for every distinct number, while the previous solution stores only occurrence counters.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

## Solution 2. Sorting

At first glance, this solution may seem a little tricky and not entirely intuitive. The key observation is that after sorting the array, all equal values become adjacent to each other.

Since the majority element appears **more than `n / 2` times**, it must occupy the middle position of the sorted array. No matter where the majority block starts, it is simply too large to avoid covering the center of the array.

```kotlin
fun majorityElement(nums: IntArray): Int {
    nums.sort()

    return nums[nums.size / 2]
}
```

**Complexity**

Time complexity: `O(n log n)`

Space complexity: `O(log n)`

## Solution 3. Boyer-Moore

The most efficient solution is the **Boyer-Moore Voting Algorithm**. At first, it may look a bit magical because we do not count all values and do not sort the array. Instead, we keep only two variables: the current `candidate` and its `count`.

The important thing to remember is that `count` is not the total number of occurrences of the `candidate`. Instead, it represents the current advantage of the `candidat`e over all other values processed so far. Whenever `count` reaches 0, the current `candidate` loses that advantage, so the next number becomes the new `candidate`.

Since the majority element appears more than `n / 2` times, it cannot be completely canceled out by all other elements. Therefore, the candidate that remains at the end is the majority element.

```kotlin
fun majorityElement(nums: IntArray): Int {
    var candidate: Int = 0
    var count: Int = 0

    for (num in nums) {
        if (count == 0) {
            candidate = num
        }

        if (candidate == num) {
            count++
        } else {
            count--
        }
    }

    return candidate
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

### Step-by-step

Let's walk through the algorithm using the following array:

```kotlin
nums = [5, 1, 5, 2, 5, 5, 4]
```

Initial values:

```text
candidate = 0
count = 0
```

| Current number | Candidate | Count | Action                                                       |
| -------------: | --------: | ----: | ------------------------------------------------------------ |
|            `5` |       `5` |   `1` | `count` is `0`, so `5` becomes the new candidate.            |
|            `1` |       `5` |   `0` | `1` is different from the candidate, so we decrease `count`. |
|            `5` |       `5` |   `1` | `count` is `0`, so `5` becomes the new candidate again.      |
|            `2` |       `5` |   `0` | `2` is different from the candidate, so we decrease `count`. |
|            `5` |       `5` |   `1` | `count` is `0`, so `5` becomes the new candidate again.      |
|            `5` |       `5` |   `2` | `5` is equal to the candidate, so we increase `count`.       |
|            `4` |       `5` |   `1` | `4` is different from the candidate, so we decrease `count`. |

At the end of the loop, `candidate` is equal to `5`, so we return it.

Let's look at another example where the candidate changes multiple times before the algorithm finds the correct majority element.

```kotlin
nums = [4, 1, 2, 3, 4, 4, 4, 4, 4]
```

Initial values:

```text
candidate = 0
count = 0
```

| Current number | Candidate | Count | Action |
|---:|---:|---:|---|
| `4` | `4` | `1` | `count` is `0`, so `4` becomes the new candidate. |
| `1` | `4` | `0` | `1` is different from the candidate, so `count` becomes `0`. |
| `2` | `2` | `1` | `count` is `0`, so `2` becomes the new candidate. |
| `3` | `2` | `0` | `3` is different from the candidate, so `count` becomes `0`. |
| `4` | `4` | `1` | `count` is `0`, so `4` becomes the new candidate again. |
| `4` | `4` | `2` | `4` matches the candidate, so `count` is increased. |
| `4` | `4` | `3` | `4` matches the candidate, so `count` is increased. |
| `4` | `4` | `4` | `4` matches the candidate, so `count` is increased. |
| `4` | `4` | `5` | `4` matches the candidate, so `count` is increased. |

Even though the candidate changed multiple times during the traversal, the algorithm still finishes with the correct majority element.
