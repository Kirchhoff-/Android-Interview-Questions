# Move Zeroes
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Array-blue)
![Patterns](https://img.shields.io/badge/Pattern-Two%20Pointers-blue)
![Patterns](https://img.shields.io/badge/Pattern-Read%20and%20Write%20Pointers-blue)

Given an integer array, move all zeroes to the end while preserving the relative order of the non-zero elements.

The array must be modified in-place without creating a copy.

### Constraints

```kotlin
1 <= nums.size <= 10^4
-2^31 <= nums[i] <= 2^31 - 1
```

### Examples

**Example 1**:

```kotlin
Input: nums = [0, 1, 0, 3, 12]
Output: [1, 3, 12, 0, 0]
```

**Example 2**:

```kotlin
Input: nums = [4, 0, 2, 0, 7]
Output: [4, 2, 7, 0, 0]
```

**Example 3**:

```kotlin
Input: nums = [0, 0, 5]
Output: [5, 0, 0]
```

## Solution 1. Overwrite Non-Zero Elements

The most straightforward approach is to overwrite the original array with all non-zero elements while skipping zeroes.

We keep a `writeIndex` that points to the next position where a non-zero value should be written. As we scan the array, every non-zero element is copied to that position, preserving the original order.

Once all non-zero elements have been moved to the beginning, every remaining position in the array is filled with `0`.

```kotlin
fun moveZeroes(nums: IntArray) {
    var writeIndex = 0

    for (readIndex in nums.indices) {
        if (nums[readIndex] != 0) {
            nums[writeIndex] = nums[readIndex]
            writeIndex++
        }
    }

    for (i in writeIndex..<nums.size) {
        nums[i] = 0
    }
}
```

### Complexity

Time complexity: `O(n)`

Space complexity: `O(1)`

### Step-by-step
Input:

```kotlin
nums = [0, 1, 0, 3, 12]
```

| Step | `readIndex` | `writeIndex` | Current value | Action | Array |
|------|------------:|-------------:|---------------|--------|-------|
| Initial | - | 0 | - | Start | `[0, 1, 0, 3, 12]` |
| 1 | 0 | 0 | `0` | Skip zero | `[0, 1, 0, 3, 12]` |
| 2 | 1 | 0 | `1` | Write `1` to index `0` | `[1, 1, 0, 3, 12]` |
| 3 | 2 | 1 | `0` | Skip zero | `[1, 1, 0, 3, 12]` |
| 4 | 3 | 1 | `3` | Write `3` to index `1` | `[1, 3, 0, 3, 12]` |
| 5 | 4 | 2 | `12` | Write `12` to index `2` | `[1, 3, 12, 3, 12]` |

After the first pass:

```kotlin
[1, 3, 12, 3, 12]
```

The remaining positions are filled with zeroes:

```kotlin
[1, 3, 12, 0, 0]
```

## Solution 2. Swap Version 1

Instead of rewriting the array, we can immediately move every non-zero element into the first available zero position.

Whenever we encounter a zero, we search for the next non-zero element and swap them. This removes the need for a second pass because every swap places one more non-zero value into its final position.

```kotlin
fun moveZeroes(nums: IntArray) {
    for (readIndex in nums.indices) {
        if (nums[readIndex] == 0) {
            var nextNonZeroIndex = readIndex

            for (findIndex in nextNonZeroIndex..<nums.size) {
                if (nums[findIndex] != 0) {
                    nextNonZeroIndex = findIndex
                    break
                }
            }

            val temp = nums[readIndex]
            nums[readIndex] = nums[nextNonZeroIndex]
            nums[nextNonZeroIndex] = temp
        }
    }
}
```

This solution works correctly, but there is one drawback. Every time a zero is found, it scans the remaining part of the array looking for the next non-zero element. As a result, the same elements may be visited many times, making this approach inefficient for large inputs.

### Complexity

Time complexity: `O(n²)`

Space complexity: `O(1)`

## Solution 3. Swap Version 2

The previous solution always starts searching for the next non-zero element from the current zero position. This means that some parts of the array may be scanned repeatedly.

We can improve this idea by remembering the index where the previous search found a non-zero element. The next search then continues from that position instead of returning to the current `readIndex`.

```kotlin
fun moveZeroes(nums: IntArray) {
    var lastNonZeroIndex = 0

    for (readIndex in nums.indices) {
        if (nums[readIndex] == 0) {
            var nextNonZeroIndex =
                if (lastNonZeroIndex == 0) readIndex else lastNonZeroIndex

            for (findIndex in nextNonZeroIndex..<nums.size) {
                if (nums[findIndex] != 0) {
                    nextNonZeroIndex = findIndex
                    lastNonZeroIndex = findIndex
                    break
                }
            }

            val temp = nums[readIndex]
            nums[readIndex] = nums[nextNonZeroIndex]
            nums[nextNonZeroIndex] = temp
        }
    }
}
```

This reduces repeated work while there are still non-zero elements ahead. However, the nested search is still present.

Once no non-zero elements remain, each subsequent zero may scan the remaining part of the array again. For example, in an array such as:

```kotlin
[1, 0, 0, 0, 0]
```

every zero searches the rest of the array but finds nothing. Therefore, the worst-case time complexity remains quadratic.

### Complexity

Time complexity: `O(n²)`

Space complexity: `O(1)`

## Solution 4. Swap (Final Version)

The previous solution still spent time searching for the next non-zero element. Instead of searching for it explicitly, we can simply process every non-zero value as soon as we encounter it.

The idea is almost identical to the overwrite solution. We still keep a `writeIndex` pointing to the first position where the next non-zero element should be placed.

The difference is that instead of overwriting the value at `writeIndex`, we swap the current element with it. As a result, every non-zero element is immediately moved into its correct position, while zeroes naturally drift toward the end of the array.

Since every element is visited exactly once and no additional searches are performed, this is the most efficient solution.

```kotlin
fun moveZeroes(nums: IntArray) {
    var writeIndex = 0

    for (readIndex in nums.indices) {
        if (nums[readIndex] != 0) {
            val temp = nums[writeIndex]
            nums[writeIndex] = nums[readIndex]
            nums[readIndex] = temp

            writeIndex++
        }
    }
}
```

### Complexity

Time complexity: `O(n)`

Space complexity: `O(1)`

### Step-by-step

`writeIndex` points to the position where the next non-zero element should be placed.

Every time `readIndex` finds a non-zero element, we swap it with the value at `writeIndex` and then move `writeIndex` one position forward. Since non-zero elements are found from left to right, their original order is preserved.

#### Example 1. Zeroes are separated

```kotlin
nums = [0, 1, 0, 2]
```

Initial value:

```text
writeIndex = 0
```

| `readIndex` | `writeIndex` | Current value | Action | Array after the step |
|---:|---:|---:|---|---|
| `0` | `0` | `0` | Skip the zero. `writeIndex` remains at `0`. | `[0, 1, 0, 2]` |
| `1` | `0` | `1` | Swap indexes `1` and `0`. Move `writeIndex` to `1`. | `[1, 0, 0, 2]` |
| `2` | `1` | `0` | Skip the zero. `writeIndex` remains at `1`. | `[1, 0, 0, 2]` |
| `3` | `1` | `2` | Swap indexes `3` and `1`. Move `writeIndex` to `2`. | `[1, 2, 0, 0]` |

The result is:

```kotlin
[1, 2, 0, 0]
```

After moving `1`, `writeIndex` becomes `1` because index `0` already contains the first non-zero element. The next non-zero element must therefore be placed at index `1`.

---

#### Example 2. Several zeroes appear in a row

```kotlin
nums = [0, 0, 0, 4, 5]
```

Initial value:

```text
writeIndex = 0
```

| `readIndex` | `writeIndex` | Current value | Action | Array after the step |
|---:|---:|---:|---|---|
| `0` | `0` | `0` | Skip the zero. `writeIndex` remains at `0`. | `[0, 0, 0, 4, 5]` |
| `1` | `0` | `0` | Skip the zero. `writeIndex` remains at `0`. | `[0, 0, 0, 4, 5]` |
| `2` | `0` | `0` | Skip the zero. `writeIndex` remains at `0`. | `[0, 0, 0, 4, 5]` |
| `3` | `0` | `4` | Swap indexes `3` and `0`. Move `writeIndex` to `1`. | `[4, 0, 0, 0, 5]` |
| `4` | `1` | `5` | Swap indexes `4` and `1`. Move `writeIndex` to `2`. | `[4, 5, 0, 0, 0]` |

The result is:

```kotlin
[4, 5, 0, 0, 0]
```

Skipping several zeroes does not change `writeIndex`. It continues pointing to index `0` until the first non-zero element is found. After `4` is placed there, the next available position becomes index `1`.

---

#### Example 3. Non-zero elements already appear at the beginning

```kotlin
nums = [1, 2, 0, 3]
```

Initial value:

```text
writeIndex = 0
```

| `readIndex` | `writeIndex` | Current value | Action | Array after the step |
|---:|---:|---:|---|---|
| `0` | `0` | `1` | Swap index `0` with itself. Move `writeIndex` to `1`. | `[1, 2, 0, 3]` |
| `1` | `1` | `2` | Swap index `1` with itself. Move `writeIndex` to `2`. | `[1, 2, 0, 3]` |
| `2` | `2` | `0` | Skip the zero. `writeIndex` remains at `2`. | `[1, 2, 0, 3]` |
| `3` | `2` | `3` | Swap indexes `3` and `2`. Move `writeIndex` to `3`. | `[1, 2, 3, 0]` |

The result is:

```kotlin
[1, 2, 3, 0]
```

When there are no zeroes before a non-zero element, `readIndex` and `writeIndex` are equal. Swapping an element with itself does not change the array, but `writeIndex` still moves forward because one more non-zero element is now in its correct position.
