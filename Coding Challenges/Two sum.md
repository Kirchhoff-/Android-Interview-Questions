# Two Sum
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Array](https://img.shields.io/badge/Topic-Array-blue)
![Hash Table](https://img.shields.io/badge/Topic-Hash%20Table-blue)

Given an array of integers `nums` and an integer `target`.

Your task is to find two different numbers whose sum equals `target` and return their indices.

You may assume that:
- exactly one valid solution exists;
- the same element cannot be used twice.

Example:

```kotlin
nums = [2, 7, 11, 15]
target = 9
```

Expected result:

```kotlin
[0, 1]
```

Because:

```kotlin
nums[0] + nums[1] = 2 + 7 = 9
```

---

Another example:

```kotlin
nums = [3, 2, 4]
target = 6
```

Expected result:

```kotlin
[1, 2]
```

Because:

```kotlin
nums[1] + nums[2] = 2 + 4 = 6
```

## Solution 1: Brute Force

The first idea is simple: take one number and compare it with every number after it.

```kotlin
fun twoSum(
    nums: IntArray,
    target: Int,
): IntArray {
    for (i in 0..<nums.size) {
        val firstNumber: Int = nums[i]

        for (j in i + 1..<nums.size) {
            if (firstNumber + nums[j] == target) {
                return intArrayOf(i, j)
            }
        }
    }

    return intArrayOf(0, 0)
}
```

### Complexity

- Time: `O(n²)`
- Space: `O(1)`

### Why this works

For each element, we check all elements after it.

Example:

```kotlin
nums = [2, 7, 11, 15]
target = 9
```

We start with `2` and compare it with the next numbers:

```kotlin
2 + 7 = 9
```

So we return:

```kotlin
[0, 1]
```

This solution works, but it checks pairs one by one, so it becomes inefficient when the array grows.

## Solution 2: Optimal Approach

The brute-force solution works, but it repeatedly checks the same combinations.

A more efficient approach is to store numbers we have already seen in a `HashMap`.

The idea is simple:

For each number, calculate what value we need to reach `target`.

```kotlin
needed = target - current
```

Then check whether that value has already been seen.

If yes, we immediately found the answer.

```kotlin
fun twoSum(
    nums: IntArray,
    target: Int,
): IntArray {
    val seen = hashMapOf<Int, Int>()

    for (index in nums.indices) {
        val current = nums[index]
        val needed = target - current

        seen[needed]?.let { previousIndex ->
            return intArrayOf(previousIndex, index)
        }

        seen[current] = index
    }

    error("No solution found")
}
```

### Complexity

- Time: `O(n)`
- Space: `O(n)`

### Why this works

Example:

```kotlin
nums = [2, 7, 11, 15]
target = 9
```

Step 1:

```kotlin
current = 2
needed = 7
```

`7` is not in the map yet.

Store:

```kotlin
2 -> 0
```

---

Step 2:

```kotlin
current = 7
needed = 2
```

`2` already exists:

```kotlin
2 -> 0
```

Return:

```kotlin
[0, 1]
```

Unlike the brute-force solution, we do not repeatedly scan the array.

Each number is processed only once.
