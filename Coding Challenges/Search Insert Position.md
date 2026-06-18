# Search Insert Position
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Binary%20Search-blue)

Given an array `nums` of **distinct** integers sorted in **ascending** order and a `target` value.

Return the index if the target is found. If the target is not found, return the index where it would be inserted to keep the array sorted.

You must write an algorithm with `O(log n)` runtime complexity.

### Constraints

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `-10^4 <= target <= 10^4`

### Examples

**Example 1**:

```kotlin
Input: nums = [1, 3, 5, 6], target = 5
Output: 2
```

Explanation:

```text
Target value 5 already exists in the array.

Index:  0  1  2  3
Value: [1, 3, 5, 6]
              ^
```

Therefore, we return index `2`.

**Example 2**:

```kotlin
Input: nums = [1, 3, 5, 6], target = 4
Output: 2
```

Explanation:

```text
Target value 4 does not exist in the array.

Index:  0  1  2  3
Value: [1, 3, 5, 6]

Insert 4 between 3 and 5:

Index:  0  1  2  3  4
Value: [1, 3, 4, 5, 6]
              ^
```

Therefore, we return index `2`.

**Example 3**:

```kotlin
Input: nums = [1, 3, 5, 6], target = 7
Output: 4
```

Explanation:

```text
Target value 7 is greater than every element in the array.

Index:  0  1  2  3
Value: [1, 3, 5, 6]

Insert 7 at the end:

Index:  0  1  2  3  4
Value: [1, 3, 5, 6, 7]
                 ^
```

Therefore, we return index `4`.

## Solution

This problem is a logical continuation of the
[Sqrt(x)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Sqrt%28x%29.md) challenge.

If you are new to Binary Search and would like a detailed explanation of the algorithm, I recommend reviewing the
[Sqrt(x)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Sqrt%28x%29.md) challenge first. In this article, we will focus only on the concepts that are unique to this problem.

---

The main idea is very similar to the previous Binary Search challenges.

We are given a sorted array, and our goal is to find either:

* the index of the `target`, if it already exists in the array;
* the position where the `target` should be inserted, if it does not exist.

At first glance, this problem may look slightly different from a regular search problem because sometimes the target is not present in the array. However, the sorted order still allows us to discard half of the search range on each step.

Before starting the Binary Search, we can handle a few boundary cases:

```kotlin
if (nums.first() >= target) return 0
if (nums.last() == target) return nums.size - 1
if (nums.last() < target) return nums.size
```

These checks cover the cases where the target should be inserted at the beginning, is equal to the last element, or should be inserted at the end.

After handling boundary cases, we can run Binary Search inside the remaining range. On each iteration, we calculate the middle index and check the corresponding value:

```kotlin
val nextCandidateIndex: Int = rangeStart + (rangeEnd - rangeStart) / 2
val number: Int = nums[nextCandidateIndex]
```

If this value is equal to `target`, we can return the current index immediately:

```kotlin
if (number == target) return nextCandidateIndex
```

Otherwise, we shrink the search range depending on whether the current value is greater or smaller than `target`.

When only two neighboring elements remain, the correct insert position is `rangeEnd`.


```kotlin
fun searchInsert(nums: IntArray, target: Int): Int {
    if (nums.first() >= target) return 0
    if (nums.last() == target) return nums.size - 1
    if (nums.last() < target) return nums.size

    var rangeStart: Int = 0
    var rangeEnd: Int = nums.size - 1

    while (true) {
        val nextCandidateIndex: Int = rangeStart + (rangeEnd - rangeStart) / 2
        val number: Int = nums[nextCandidateIndex]

        if (number == target) return nextCandidateIndex

        if (number > target) {
            rangeEnd = nextCandidateIndex
        } else {
            rangeStart = nextCandidateIndex
        }

        if (rangeEnd - rangeStart == 1) {
            return rangeEnd
        }
    }
}
```

**Complexity**

Time complexity: `O(log n)`

Space complexity: `O(1)`

## Step-by-Step Example

Let's use the following input:

```kotlin
nums = [1, 3, 5, 7, 9, 11, 13, 15]
target = 8
```

We start with:

```text
Index:  0  1  2  3  4   5   6   7
Value: [1, 3, 5, 7, 9, 11, 13, 15]

rangeStart = 0
rangeEnd = 7
```

### Iteration 1

```text
nextCandidateIndex = 0 + (7 - 0) / 2 = 3
number = 7
```

Since `7 < 8`, we move the left boundary:

```text
rangeStart = 3
rangeEnd = 7
```

### Iteration 2

```text
nextCandidateIndex = 3 + (7 - 3) / 2 = 5
number = 11
```

Since `11 > 8`, we move the right boundary:

```text
rangeStart = 3
rangeEnd = 5
```

### Iteration 3

```text
nextCandidateIndex = 3 + (5 - 3) / 2 = 4
number = 9
```

Since `9 > 8`, we move the right boundary again:

```text
rangeStart = 3
rangeEnd = 4
```

Now only two neighboring elements remain:

```text
Index:  0  1  2  3  4   5   6   7
Value: [1, 3, 5, 7, 9, 11, 13, 15]
                ^  ^
                |  |
       rangeStart  rangeEnd
```

`rangeStart` points to `7`, which is smaller than the target.

`rangeEnd` points to `9`, which is greater than the target.

Therefore, the correct insert position is:

```kotlin
return rangeEnd
```

Result:

```text
Output: 4
```
