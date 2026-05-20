# Minimum Common Value
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Two%20Pointers-blue)

Given two integer arrays sorted in non-decreasing order, find the smallest value that appears in both arrays. A value is considered common if it appears at least once in each array.

If no common value exists, return `-1`.

**Example 1**
```
Input:
nums1 = [1, 2, 3]
nums2 = [2, 4]

Output:
2
```

**Example 2**
```
Input:
nums1 = [1, 3, 5]
nums2 = [2, 4, 6]

Output:
-1
```

## Solution 1: `Set`

The simplest way to solve this problem is to convert one of the arrays to a `Set` and then iterate over the second array.

Since the arrays are sorted in non-decreasing order, the first matching value we encounter in the second array will be the smallest common value.
```
fun getCommon(nums1: IntArray, nums2: IntArray): Int {
    val nums1Set = nums1.toSet()

    nums2.forEach { number ->
        if (number in nums1Set) {
            return number
        }
    }

    return -1
}
```

**Why use a `Set` here?**

Without a set, we would need to repeatedly search for values from one array in the other. Using a set gives us fast lookup, so after converting one array, we can simply iterate over the second one and return the first matching value.

**Complexity**:

- `n` - size of `nums1`

- `m` - size of `nums2`

**Time complexity**: `O(n + m)`

- `O(n)` to convert `nums1` into a set
- up to `O(m)` to iterate over `nums2`

**Space complexity**: `O(n)`

## Solution 2: Two Pointers

A better solution is to take advantage of the fact that both arrays are already sorted.

Instead of creating an additional collection, we can keep two pointers:
- one pointer for `nums1`;
- one pointer for `nums2`.

At each step, we compare the current values:
- if they are equal, we found the smallest common value;
- if the value from `nums1` is smaller, we move the first pointer;
- if the value from `nums2` is smaller, we move the second pointer.

```
fun getCommon(nums1: IntArray, nums2: IntArray): Int {
    var firstIndex = 0
    var secondIndex = 0

    while (firstIndex < nums1.size && secondIndex < nums2.size) {
        val firstValue = nums1[firstIndex]
        val secondValue = nums2[secondIndex]

        when {
            firstValue == secondValue -> return firstValue
            firstValue < secondValue -> firstIndex++
            else -> secondIndex++
        }
    }

    return -1
}
```

The main idea is simple: when the values are different, we move forward in the array with the smaller value. Because both arrays are sorted, all next values in the same array will only get larger. This means the smaller value will never match the current larger one, so we can safely move its pointer forward.

**Complexity:**

- `n` - size of `nums1`
- `m` - size of `nums2`

**Time complexity:** `O(n + m)`

In the worst case, we iterate through both arrays once.

**Space complexity:** `O(1)`

Unlike the previous solution, we do not create additional collections and only use two pointers.

## Alternative Solutions

### Binary Search

Since both arrays are sorted, we can iterate over one array and use binary search to check whether the current value exists in the other array.

```kotlin
fun getCommon(nums1: IntArray, nums2: IntArray): Int {
    nums1.forEach { value ->
        if (nums2.binarySearch(value) >= 0) {
            return value
        }
    }

    return -1
}
```

**Complexity:**

- `n` - size of `nums1`
- `m` - size of `nums2`

**Time complexity:** `O(n log m)`

We iterate through the first array and perform binary search in the second one for each value.

**Space complexity:** `O(1)`

### Set Intersection

A more concise solution is to convert both arrays to sets and use built-in collection operations.

```kotlin
fun getCommon(nums1: IntArray, nums2: IntArray): Int {
    return nums1
        .toSet()
        .intersect(nums2.toSet())
        .minOrNull()
        ?: -1
}
```

This solution is compact and easy to write, but it hides the algorithmic details and creates additional collections.

**Complexity:**

- `n` - size of `nums1`
- `m` - size of `nums2`

**Time complexity:** `O(n + m)`

**Space complexity:** `O(n + m)`

### Brute Force

The most straightforward approach is to compare every value from the first array with every value from the second one.

```kotlin
fun getCommon(nums1: IntArray, nums2: IntArray): Int {
    nums1.forEach { first ->
        nums2.forEach { second ->
            if (first == second) {
                return first
            }
        }
    }

    return -1
}
```

This approach is simple but inefficient because it does not use the fact that arrays are sorted.

**Complexity:**

- `n` - size of `nums1`
- `m` - size of `nums2`

**Time complexity:** `O(n * m)`

**Space complexity:** `O(1)`
