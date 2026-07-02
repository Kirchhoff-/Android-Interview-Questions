# Third Maximum Number
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Array-blue)

Given an integer array `nums`, return the **third distinct maximum** number in the array.

If the third distinct maximum does not exist, return the largest number instead.

### Constraints

```kotlin
1 <= nums.size <= 10^4
-2^31 <= nums[i] <= 2^31 - 1
```

### Examples

**Example 1**:

```kotlin
Input: nums = [3, 2, 1]
Output: 1
```

**Example 2**:

```kotlin
Input: nums = [1, 2]
Output: 2
```

**Example 3**:

```kotlin
Input: nums = [2, 2, 3, 1]
Output: 1
```

## Solution

There are several ways to solve this problem. For example, we could sort the array or use a `TreeSet` to keep only distinct values.

However, these approaches are not optimal. Sorting adds extra `O(n log n)` time complexity, and a `TreeSet` also adds unnecessary overhead. We only need to keep track of the three largest distinct values while traversing the array once.

We use three nullable variables:

```kotlin
firstMax
secondMax
thirdMax
```

Each variable stores one of the three largest distinct values found so far. If the current number is already equal to one of them, we simply skip it because duplicates should not be counted.

Notice that these variables are nullable instead of being initialized with `Int.MIN_VALUE`. Since the input may contain any valid `Int` value, including `Int.MIN_VALUE`, using it as a placeholder could produce incorrect results for some edge cases. Using `null` clearly indicates that the corresponding maximum has not been found yet.

When a new maximum is found, the existing maximums are shifted down to preserve their order.

```kotlin
fun thirdMax(nums: IntArray): Int {
    var firstMax: Int? = null
    var secondMax: Int? = null
    var thirdMax: Int? = null

    for (num in nums) {
        if (num == firstMax || num == secondMax || num == thirdMax) {
            continue
        }

        if (firstMax == null || num > firstMax) {
            thirdMax = secondMax
            secondMax = firstMax
            firstMax = num
        } else if (secondMax == null || num > secondMax) {
            thirdMax = secondMax
            secondMax = num
        } else if (thirdMax == null || num > thirdMax) {
            thirdMax = num
        }
    }

    return thirdMax ?: firstMax!!
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

### Step-by-step

Let's walk through the algorithm using the following array:

```kotlin
nums = [2, 2, 3, 1]
```

Initial values:

```text
firstMax = null
secondMax = null
thirdMax = null
```

| Current number | First max | Second max | Third max | Action |
|---:|---:|---:|---:|---|
| `2` | `2` | `null` | `null` | `firstMax` is `null`, so `2` becomes the first maximum. |
| `2` | `2` | `null` | `null` | `2` is already stored as `firstMax`, so we skip it. |
| `3` | `3` | `2` | `null` | `3` is greater than `firstMax`, so previous maximums are shifted down. |
| `1` | `3` | `2` | `1` | `1` becomes the third maximum. |

At the end of the loop, `thirdMax` is equal to `1`, so we return it.

Now let's consider a case where the third distinct maximum does not exist:
```kotlin
nums = [1, 2]
```

Initial values:

```text
firstMax = null
secondMax = null
thirdMax = null
```

| Current number | First max | Second max | Third max | Action |
|---:|---:|---:|---:|---|
| `1` | `1` | `null` | `null` | `firstMax` is `null`, so `1` becomes the first maximum. |
| `2` | `2` | `1` | `null` | `2` is greater than `firstMax`, so previous maximums are shifted down. |

At the end of the loop, `thirdMax` is still `null`. It means that the third distinct maximum does not exist, so we return `firstMax`, which is `2`.
