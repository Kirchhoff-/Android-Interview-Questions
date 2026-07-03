# Remove Element
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Array-blue)
![Patterns](https://img.shields.io/badge/Pattern-Two%20Pointers-blue)
![Patterns](https://img.shields.io/badge/Pattern-Read%20and%20Write%20Pointers-blue)

Given an integer array `nums` and an integer `targetValue`, remove all occurrences of `targetValue` from the array **in-place**. The order of the remaining elements does not matter.

Return the number of elements that remain after all occurrences of `targetValue` have been removed. Let this number be `k`.

After the function returns, the first `k` elements of `nums` should contain all values that are not equal to `targetValue`. The remaining elements are not important.

### Constraints

```kotlin
0 <= nums.size <= 100
0 <= nums[i] <= 50
0 <= targetValue <= 100
```

### Examples

**Example 1**:

```kotlin
Input: nums = [1, 2, 1, 3], targetValue = 1
Output: 2, nums = [2, 3, _, _]
```

Explanation:

The function should return `2`, since there are two elements that are not equal to `targetValue`.

The first two elements of `nums` should contain `2` and `3`.

**Example 2**:

```kotlin
Input: nums = [4, 4, 4], targetValue = 4
Output: 0, nums = [_, _, _]
```

Explanation:

Every element is equal to `targetValue`, so the function should return `0`.

Since no elements remain, every position in the array is ignored.

## Solution 1. Filtering

In real production code, we would probably solve this problem by creating a new filtered array first.

We can filter out all values equal to `targetValue`, copy the remaining values back to the original array, and return the size of the filtered array.

This approach clearly separates the solution into two steps: first we decide which elements should remain, and then we write them back to `nums`.

```kotlin
fun removeElement(nums: IntArray, targetValue: Int): Int {
    val filteredArray = nums.filterNot { it == targetValue }.toIntArray()

    for (i in filteredArray.indices) {
        nums[i] = filteredArray[i]
    }

    return filteredArray.size
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

## Solution 2. Two Indexes

The filtering solution is simple and easy to understand, but it creates an additional array.

In an interview, this is usually not what the interviewer expects. Since the problem explicitly asks us to modify the array **in-place**, the expected solution is to avoid creating another array and rewrite `nums` directly.

We can use two indexes for this: one index to read elements from the array and another index to track where the next valid element should be written.

If the current element is equal to `targetValue`, we skip it. If it is not equal to `targetValue`, we write it at `writeIndex` and move `writeIndex` forward.

```kotlin
fun removeElement(nums: IntArray, targetValue: Int): Int {
    var writeIndex = 0

    for (i in nums.indices) {
        val currentElement = nums[i]

        if (currentElement != targetValue) {
            nums[writeIndex] = currentElement
            writeIndex++
        }
    }

    return writeIndex
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

## Step-by-step

Let's walk through the algorithm using the following array:

```kotlin
nums = [1, 2, 1, 3]
targetValue = 1
```

Initial value:

```text
writeIndex = 0
```

| Current index | Current element | writeIndex | Action | Array |
|---:|---:|---:|---|---|
| `0` | `1` | `0` | `1` is equal to `targetValue`, so we skip it. | `[1, 2, 1, 3]` |
| `1` | `2` | `0` | `2` is not equal to `targetValue`, so we write it at index `0` and move `writeIndex` to `1`. | `[2, 2, 1, 3]` |
| `2` | `1` | `1` | `1` is equal to `targetValue`, so we skip it. | `[2, 2, 1, 3]` |
| `3` | `3` | `1` | `3` is not equal to `targetValue`, so we write it at index `1` and move `writeIndex` to `2`. | `[2, 3, 1, 3]` |

At the end of the loop, `writeIndex` is equal to `2`, so we return it.

The first `2` elements of `nums` contain all values that are not equal to `targetValue`:

```kotlin
[2, 3, _, _]
```

Let's look at another example:

```kotlin
nums = [4, 1, 2, 4, 3, 4, 5]
targetValue = 4
```

Initial value:

```text
writeIndex = 0
```

| Current index | Current element | writeIndex | Action | Array |
|---:|---:|---:|---|---|
| `0` | `4` | `0` | `4` is equal to `targetValue`, so we skip it. | `[4, 1, 2, 4, 3, 4, 5]` |
| `1` | `1` | `0` | `1` is not equal to `targetValue`, so we write it at index `0` and move `writeIndex` to `1`. | `[1, 1, 2, 4, 3, 4, 5]` |
| `2` | `2` | `1` | `2` is not equal to `targetValue`, so we write it at index `1` and move `writeIndex` to `2`. | `[1, 2, 2, 4, 3, 4, 5]` |
| `3` | `4` | `2` | `4` is equal to `targetValue`, so we skip it. | `[1, 2, 2, 4, 3, 4, 5]` |
| `4` | `3` | `2` | `3` is not equal to `targetValue`, so we write it at index `2` and move `writeIndex` to `3`. | `[1, 2, 3, 4, 3, 4, 5]` |
| `5` | `4` | `3` | `4` is equal to `targetValue`, so we skip it. | `[1, 2, 3, 4, 3, 4, 5]` |
| `6` | `5` | `3` | `5` is not equal to `targetValue`, so we write it at index `3` and move `writeIndex` to `4`. | `[1, 2, 3, 5, 3, 4, 5]` |

At the end of the loop, `writeIndex` is equal to `4`, so we return it.

The first `4` elements of `nums` contain all values that are not equal to `targetValue`:

```kotlin
[1, 2, 3, 5, _, _, _]
```
