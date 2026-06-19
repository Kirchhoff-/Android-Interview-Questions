# Contains Duplicate
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Hash%20Set-blue)

Given an integer array `nums`, return `true` if any value appears more than once in the array, and return `false` if all elements are unique.

### Constraints

```kotlin
1 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
```

### Examples

**Example 1**:

```kotlin
Input: nums = [1, 2, 3, 1]
Output: true
```

The value 1 appears more than once.

**Example 2**:

```kotlin
Input: nums = [1, 2, 3, 4]
Output: false
```

Every value appears exactly once.

**Example 3**:

```kotlin
Input: nums = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2]
Output: true
```

Multiple values appear more than once.

## Solution

The first solution is the classic brute-force approach. We can simply compare every element with every other element in the array.

```kotlin
fun containsDuplicate(nums: IntArray): Boolean {
    for (i in nums.indices) {
        for (j in i + 1 until nums.size) {
            if (nums[i] == nums[j]) {
                return true
            }
        }
    }

    return false
}
```

**Complexity**

Time complexity: `O(n²)`

Space complexity: `O(1)`

This solution works, but it is not efficient. For every element, we may need to compare it with almost every other element. As the array grows, the number of comparisons grows very quickly.

A more efficient approach is to use a `Set`. A `Set` is a collection that stores only unique values. If the array contains duplicate values, converting it to a `Set` will automatically remove them.

After that, we can compare the size of the original array with the size of the resulting `Set`.

```kotlin
fun containsDuplicate(nums: IntArray): Boolean {
    return nums.toSet().size != nums.size
}
```

If both sizes are equal, all values are unique. If the `Set` is smaller, at least one duplicate existed in the original array.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`

This solution is concise, readable, and would be perfectly acceptable in most real-world projects where simplicity is preferred over micro-optimizations.

However, during interviews, it is often better to demonstrate how the solution works internally. Instead of relying on `toSet()`, we can create a `HashSet` and process the values one by one.

```kotlin
fun containsDuplicate(nums: IntArray): Boolean {
    val seen = mutableSetOf<Int>()

    for (num in nums) {
        if (!seen.add(num)) {
            return true
        }
    }

    return false
}
```

The key detail is that `add()` returns:

```text
true  -> value was added successfully
false -> value already exists in the set
```

So if `add()` returns `false`, we know that this value has already been seen before. That means we found a duplicate and can immediately return `true`.

This gives us the same asymptotic complexity as the `toSet()` solution, but with one important advantage: the algorithm can stop as soon as a duplicate is found.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(n)`
