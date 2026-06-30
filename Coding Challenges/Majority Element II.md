# Majority Element II
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-gold)
![Pattern](https://img.shields.io/badge/Pattern-Array-blue)
![Pattern](https://img.shields.io/badge/Pattern-Hash%20Map-blue)

Given an integer array `nums`, return all elements that appear more than `n / 3` times.

### Constraints

```kotlin
1 <= nums.length <= 5 * 10^4
-10^9 <= nums[i] <= 10^9
```

### Examples

**Example 1**:

```kotlin
Input: nums = [4, 2, 4]
Output: [4]
```

`4` appears 2 times, which is greater than `3 / 3 = 1`.

**Example 2**:

```kotlin
Input: nums = [1]
Output: [1]
```

The array contains only one element, so it is the result.

**Example 3**:

```kotlin
Input: nums = [6, 6, 7, 7, 7, 6, 8]
Output: [6, 7]
```

Both `6` and `7` appear 3 times, which is greater than `7 / 3 = 2`.

## Solution 1. HashMap

Overall, the idea is exactly the same as in the simpler version of this problem: **[Majority Element](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Majority%20Element.md)**.

The most straightforward way to solve this problem is to count how many times each value appears in the array.

We can use a `HashMap`, where the key is the number from the array and the value is the number of its occurrences. Once all frequencies are calculated, we simply return every number that appears more than `n / 3` times.

```kotlin
fun majorityElement(nums: IntArray): List<Int> {
    val frequencyMap: MutableMap<Int, Int> = mutableMapOf()
    val requiredFrequency: Int = nums.size / 3

    for (num in nums) {
        if (frequencyMap.contains(num)) {
            frequencyMap[num] = frequencyMap.getValue(num) + 1
        } else {
            frequencyMap[num] = 1
        }
    }

    return frequencyMap
        .filterValues { it > requiredFrequency }
        .keys
        .toList()
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

A similar solution can also be implemented using Kotlin standard library functions.

Instead of manually maintaining a `HashMap`, we can use `groupingBy()` together with `eachCount()` to calculate the frequency of every element. Once all frequencies are calculated, we simply filter all elements that appear more than `n / 3` times and return the remaining keys.

```kotlin
fun majorityElement(nums: IntArray): List<Int> =
    nums
        .toList()
        .groupingBy { it }
        .eachCount()
        .filterValues { it > nums.size / 3 }
        .keys
        .toList()
```

Although this solution is much shorter, it follows exactly the same counting-based approach as the `HashMap` implementation above. The main difference is that `groupingBy().eachCount()` performs the counting internally.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

## Solution 2. Boyer-Moore

The optimized solution is based on the **Boyer-Moore Voting Algorithm**, but this time we need to keep two candidates instead of one.

The key observation is that there can be at most two elements that appear more than `n / 3` times. If three different elements appeared more than `n / 3` times, their total number of occurrences would be greater than `n`, which is impossible.

During the first pass, we do not count the real frequency of each element. Instead, we only search for two possible candidates. When the current number matches one of the candidates, we increase its vote count. When there is an empty candidate slot, we use the current number as a new candidate. If the current number is different from both candidates and both vote counts are positive, we decrease both vote counts.

After the first pass, `candidate1` and `candidate2` are only potential answers. We still need a second pass to calculate their real frequencies and check whether they actually appear more than `n / 3` times.

```kotlin
fun majorityElement(nums: IntArray): List<Int> {
    if (nums.size == 1) return nums.toList()

    var candidate1: Int = 0
    var candidate1Votes: Int = 0

    var candidate2: Int = 0
    var candidate2Votes: Int = 0

    val requiredFrequency: Int = nums.size / 3

    for (num in nums) {
        when {
            num == candidate1 -> candidate1Votes++
            num == candidate2 -> candidate2Votes++
            candidate1Votes == 0 -> {
                candidate1 = num
                candidate1Votes = 1
            }
            candidate2Votes == 0 -> {
                candidate2 = num
                candidate2Votes = 1
            }
            else -> {
                candidate1Votes--
                candidate2Votes--
            }
        }
    }

    var candidate1Frequency: Int = 0
    var candidate2Frequency: Int = 0

    for (num in nums) {
        when (num) {
            candidate1 -> candidate1Frequency++
            candidate2 -> candidate2Frequency++
        }
    }

    return buildList {
        if (candidate1Frequency > requiredFrequency) add(candidate1)
        if (candidate2Frequency > requiredFrequency) add(candidate2)
    }
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

### Step-by-step

Let's walk through the algorithm using the following array:

```kotlin
nums = [6, 6, 7, 7, 7, 6, 8]
```

Initial values:

```text
candidate1 = 0
candidate1Votes = 0
candidate2 = 0
candidate2Votes = 0
```

| Current number | Candidate 1 | Votes 1 | Candidate 2 | Votes 2 | Action |
|---:|---:|---:|---:|---:|---|
| `6` | `6` | `1` | `0` | `0` | `candidate1Votes` is `0`, so `6` becomes the first candidate. |
| `6` | `6` | `2` | `0` | `0` | `6` matches the first candidate, so `candidate1Votes` is increased. |
| `7` | `6` | `2` | `7` | `1` | `candidate2Votes` is `0`, so `7` becomes the second candidate. |
| `7` | `6` | `2` | `7` | `2` | `7` matches the second candidate, so `candidate2Votes` is increased. |
| `7` | `6` | `2` | `7` | `3` | `7` matches the second candidate, so `candidate2Votes` is increased. |
| `6` | `6` | `3` | `7` | `3` | `6` matches the first candidate, so `candidate1Votes` is increased. |
| `8` | `6` | `2` | `7` | `2` | `8` matches neither candidate, so both vote counts are decreased. |

After the first pass, the possible candidates are `6` and `7`. However, these vote counts are not real frequencies, so we need to count how many times both candidates actually appear in the array.

```text
6 appears 3 times
7 appears 3 times
```

Since `n = 7` and `n / 3 = 2`, both `6` and `7` appear more than `n / 3` times, so the result is:

```kotlin
[6, 7]
```

Let's look at another example where the first pass leaves us with candidates, but not all of them satisfy the final condition:

```kotlin
nums = [1, 2, 2, 3]
```

Initial values:

```text
candidate1 = 0
candidate1Votes = 0
candidate2 = 0
candidate2Votes = 0
```

| Current number | Candidate 1 | Votes 1 | Candidate 2 | Votes 2 | Action |
|---:|---:|---:|---:|---:|---|
| `1` | `1` | `1` | `0` | `0` | `candidate1Votes` is `0`, so `1` becomes the first candidate. |
| `2` | `1` | `1` | `2` | `1` | `candidate2Votes` is `0`, so `2` becomes the second candidate. |
| `2` | `1` | `1` | `2` | `2` | `2` matches the second candidate, so `candidate2Votes` is increased. |
| `3` | `1` | `0` | `2` | `1` | `3` matches neither candidate, so both vote counts are decreased. |

After the first pass, the possible candidates are `1` and `2`. However, these vote counts are not real frequencies, so we need to count how many times both candidates actually appear in the array.

```text
1 appears 1 time
2 appears 2 times
```

Since `n = 4` and `n / 3 = 1`, only `2` appears more than `n / 3` times. The result is:

```kotlin
[2]
```
