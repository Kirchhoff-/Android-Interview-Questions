# Summary Ranges
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Array-blue)

Given a sorted array of unique integers `nums`, return a list of ranges that represents all numbers in the array.

Consecutive numbers should be grouped into a single range, while numbers that are not adjacent should start a new range.

- A range containing multiple numbers should be represented as `"start->end"`;
- A range containing a single number should be represented as `"value"`.

### Constraints

```kotlin
0 <= nums.size <= 20
-2^31 <= nums[i] <= 2^31 - 1
nums is sorted in ascending order
All values in nums are unique
```

### Examples

**Example 1**:

```kotlin
Input: nums = [1, 2, 3, 6, 8, 9]
Output: ["1->3", "6", "8->9"]
```

**Example 2**:

```kotlin
Input: nums = [-5, -4, -2, 0, 1, 2]
Output: ["-5->-4", "-2", "0->2"]
```

**Example 3**:

```kotlin
Input: nums = [7]
Output: ["7"]
```

## Solution

This problem has the `Easy` badge for a reason. There are no hidden tricks, special algorithms, or advanced data structures here. We only need to carefully transform one representation of data into another: from a sorted array of numbers into a list of range strings.

The main idea is simple: we keep the start index of the current range and iterate through the array. Once the current range ends, we add it to the result and start a new range from the next element.

There are two small tricky parts in this problem.

- We need to handle the last element correctly. Usually, we compare the current element with the next one, but the last element has no next element. 

- We should be careful when checking if two numbers are consecutive. Writing `nums[index] + 1 != nums[index + 1]` may cause overflow when `nums[index]` is equal to `Int.MAX_VALUE`.

Putting everything together, the final implementation looks like this:

```kotlin
fun summaryRanges(nums: IntArray): List<String> {
    val result = mutableListOf<String>()
    var rangeStartIndex = 0

    for (index in nums.indices) {
        val isLastElement = index == nums.lastIndex
        val isRangeEnd = isLastElement || nums[index + 1] - nums[index] != 1

        if (isRangeEnd) {
            if (rangeStartIndex == index) {
                result.add(nums[index].toString())
            } else {
                result.add("${nums[rangeStartIndex]}->${nums[index]}")
            }

            rangeStartIndex = index + 1
        }
    }

    return result
}
```

Now let's see how the implementation handles both tricky parts.

The first one is handling the last element. Since the last number has no next element to compare against, we explicitly check whether the current index is the last index:

```kotlin
val isLastElement = index == nums.lastIndex
val isRangeEnd = isLastElement || nums[index + 1] - nums[index] != 1
```

Thanks to Kotlin's short-circuit evaluation (`||`), `nums[index + 1]` is never accessed when `index` is equal to `lastIndex`, preventing an `IndexOutOfBoundsException`.

The second one is checking whether two numbers are consecutive. Instead of writing:

```kotlin
nums[index] + 1 == nums[index + 1]
```

we compare the difference between the numbers:

```kotlin
nums[index + 1] - nums[index] == 1
```

This avoids a potential integer overflow when `nums[index]` is equal to `Int.MAX_VALUE`.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

### Step-by-step

Let's walk through the algorithm using the examples from above.

**Example 1**:

```kotlin
nums = [1, 2, 3, 6, 8, 9]
```

Initial values:

```text
result = []
rangeStartIndex = 0
```

| Index | Current number | Is range end? | Added to result | New rangeStartIndex |
|---:|---:|---|---|---:|
| `0` | `1` | No, because `2 - 1 == 1` | - | `0` |
| `1` | `2` | No, because `3 - 2 == 1` | - | `0` |
| `2` | `3` | Yes, because `6 - 3 != 1` | `"1->3"` | `3` |
| `3` | `6` | Yes, because `8 - 6 != 1` | `"6"` | `4` |
| `4` | `8` | No, because `9 - 8 == 1` | - | `4` |
| `5` | `9` | Yes, because it is the last element | `"8->9"` | `6` |

Final result:

```kotlin
["1->3", "6", "8->9"]
```

**Example 2**:

```kotlin
nums = [-5, -4, -2, 0, 1, 2]
```

Initial values:

```text
result = []
rangeStartIndex = 0
```

| Index | Current number | Is range end? | Added to result | New rangeStartIndex |
|---:|---:|---|---|---:|
| `0` | `-5` | No, because `-4 - (-5) == 1` | - | `0` |
| `1` | `-4` | Yes, because `-2 - (-4) != 1` | `"-5->-4"` | `2` |
| `2` | `-2` | Yes, because `0 - (-2) != 1` | `"-2"` | `3` |
| `3` | `0` | No, because `1 - 0 == 1` | - | `3` |
| `4` | `1` | No, because `2 - 1 == 1` | - | `3` |
| `5` | `2` | Yes, because it is the last element | `"0->2"` | `6` |

Final result:

```kotlin
["-5->-4", "-2", "0->2"]
```

**Example 3**:

```kotlin
nums = [7]
```

Initial values:

```text
result = []
rangeStartIndex = 0
```

| Index | Current number | Is range end? | Added to result | New rangeStartIndex |
|---:|---:|---|---|---:|
| `0` | `7` | Yes, because it is the last element | `"7"` | `1` |

Final result:

```kotlin
["7"]
```
