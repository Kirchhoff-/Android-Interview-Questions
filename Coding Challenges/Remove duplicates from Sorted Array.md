# Remove duplicates from Sorted Array
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Array-blue)
![Patterns](https://img.shields.io/badge/Pattern-Two%20Pointers-blue)
![Patterns](https://img.shields.io/badge/Pattern-Read%20and%20Write%20Pointers-blue)

Given a sorted integer array, remove duplicate values in-place.

After the operation, the beginning of the array should contain only unique values in sorted order. Return the number of unique elements stored in the array.

Any values beyond the returned length can be ignored.

## Constraints
* `1 <= nums.length <= 30,000`
* `-100 <= nums[i] <= 100`
* `nums` is sorted in **non-decreasing** order

## Examples
### Example 1

Input: `nums = [1, 1, 2]`

Output: `2`

Explanation:

After removing duplicates, the first 2 elements of the array are:

`[1, 2]`

### Example 2

Input: `nums = [1, 1, 1, 1]`

Output: `1`

Explanation:

All elements are the same, so only one unique value remains:

`[1]`

### Example 3

Input: `nums = [0, 0, 1, 2, 2, 3]`

Output: `4`

Explanation:

After removing duplicates, the first 4 elements of the array are:

`[0, 1, 2, 3]`

## Solution

Since the array is already sorted, all duplicate values are placed next to each other.

We can use the **Read and Write Pointers** technique:

* `readIndex` scans the array from left to right.
* `writeIndex` points to the position where the next unique value should be written.

The first element is always unique, so we start writing from index `1`.
Whenever we find a value different from the last written value, we write it to the current `writeIndex` position and move `writeIndex` forward.

```kotlin
fun removeDuplicates(nums: IntArray): Int {
    var writeIndex: Int = 1
    var lastWrittenValue: Int = nums[0]

    for (readIndex in 1..<nums.size) {
        val currentItem: Int = nums[readIndex]

        if (lastWrittenValue != currentItem) {
            nums[writeIndex] = currentItem
            lastWrittenValue = currentItem
            writeIndex++
        }
    }

    return writeIndex
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

## Step-by-step example

Let's use the following array:

```kotlin
nums = [0, 0, 1, 2, 2, 3]
```

Initial state:

```kotlin
writeIndex = 1
lastWrittenValue = 0
```

The first element is already treated as unique, so we start reading from index `1`.

```text
readIndex = 1, currentItem = 0
0 is equal to lastWrittenValue, so we skip it.

nums = [0, 0, 1, 2, 2, 3]
writeIndex = 1
```

```text
readIndex = 2, currentItem = 1
1 is different from lastWrittenValue, so we write it at index 1.

nums = [0, 1, 1, 2, 2, 3]
writeIndex = 2
lastWrittenValue = 1
```

```text
readIndex = 3, currentItem = 2
2 is different from lastWrittenValue, so we write it at index 2.

nums = [0, 1, 2, 2, 2, 3]
writeIndex = 3
lastWrittenValue = 2
```

```text
readIndex = 4, currentItem = 2
2 is equal to lastWrittenValue, so we skip it.

nums = [0, 1, 2, 2, 2, 3]
writeIndex = 3
```

```text
readIndex = 5, currentItem = 3
3 is different from lastWrittenValue, so we write it at index 3.

nums = [0, 1, 2, 3, 2, 3]
writeIndex = 4
lastWrittenValue = 3
```

Result:

```kotlin
return 4
```

The first `4` elements contain all unique values:

```kotlin
[0, 1, 2, 3]
```

The remaining values can be ignored.
