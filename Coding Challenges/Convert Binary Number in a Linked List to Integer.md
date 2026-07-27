# Convert Binary Number in a Linked List to Integer
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)
![Pattern](https://img.shields.io/badge/Pattern-Bit%20Manipulation-blue)

You are given the `head` of a singly linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Each node stores either `0` or `1`. Together, all nodes form the binary representation of a number, where the `head` contains the most significant bit.

Return the decimal value of the binary number represented by the linked list.

## Constraints

- The number of nodes is in the range `[1, 30]`.
- `0 <= value <= 1`

## Examples

**Example 1**

```text
Input:

1 -> 0 -> 1 -> 1

Output:

11

Explanation:

The linked list represents the binary number `1011₂`, which is equal to `11₁₀`.
```

**Example 2**

```text
Input:

1 -> 0 -> 0 -> 0

Output:

8

Explanation:

The linked list represents the binary number `1000₂`, which is equal to `8₁₀`.
```

**Example 3**

```text
Input:

0 -> 1 -> 1 -> 0

Output:

6

Explanation:

The linked list represents the binary number `0110₂`, which is equal to `6₁₀`.
```

## Solution

The main nuance of this problem is that the `head` contains the most significant bit. In other words, we read the binary number from left to right.

Our first idea might be to calculate the contribution of every bit using powers of two. If the least significant bit were stored at the beginning of the list, this approach would be straightforward because every next node would correspond to the next power of two.

However, the bits are stored in the opposite order. We don't know the power of the current bit until we've already reached the end of the list.

A better approach is to build the decimal value while traversing the list. Instead of calculating the contribution of every bit separately, we process the number as we read it—from left to right.

Suppose we've already read the first bit:

```text
1
```

Its decimal value is `1`.

Now we read the next bit, `0`.

```text
10
```

What changed?

The previous `1` is no longer in the ones place. It moved to the twos place, so its contribution doubled.

```text
1  ->  2
```

Finally, we append the new bit.

```text
2 + 0 = 2
```

Let's continue.

We read another `0`.

```text
100
```

Again, every previously processed bit shifts one position to the left, so the current value doubles.

```text
2 -> 4
```

Then we append the new bit.

```text
4 + 0 = 4
```

This is exactly the same operation we perform for every node:

```text
result = result * 2 + current.value
```

No matter how long the linked list is, we repeat these two steps:

1. Double the current result.
2. Add the current bit.

The implementation is straightforward. We traverse the linked list once, apply the formula for every node, and return the final result.

```kotlin
fun getDecimalValue(head: ListNode?): Int {
    var current = head
    var result = 0

    while (current != null) {
        result = result * 2 + current.value
        current = current.next
    }

    return result
}
```

### Complexity

**Time Complexity:** `O(n)`

We visit every node exactly once.

**Space Complexity:** `O(1)`

We only use two variables (`current` and `result`), regardless of the size of the linked list.

## Step-by-Step

### Example 1

```text
1 -> 0 -> 1
```

We start with:

```text
result = 0
```

| Step | Current bit | Calculation | Result |
|---:|---:|---|---:|
| 1 | `1` | `0 * 2 + 1` | `1` |
| 2 | `0` | `1 * 2 + 0` | `2` |
| 3 | `1` | `2 * 2 + 1` | `5` |

The linked list represents:

```text
101₂ = 5₁₀
```

### Example 2

```text
1 -> 0 -> 0 -> 0
```

We start with:

```text
result = 0
```

| Step | Current bit | Calculation | Result |
|---:|---:|---|---:|
| 1 | `1` | `0 * 2 + 1` | `1` |
| 2 | `0` | `1 * 2 + 0` | `2` |
| 3 | `0` | `2 * 2 + 0` | `4` |
| 4 | `0` | `4 * 2 + 0` | `8` |

The linked list represents:

```text
1000₂ = 8₁₀
```

### Example 3

```text
1 -> 1 -> 0 -> 1 -> 1
```

We start with:

```text
result = 0
```

| Step | Current bit | Calculation | Result |
|---:|---:|---|---:|
| 1 | `1` | `0 * 2 + 1` | `1` |
| 2 | `1` | `1 * 2 + 1` | `3` |
| 3 | `0` | `3 * 2 + 0` | `6` |
| 4 | `1` | `6 * 2 + 1` | `13` |
| 5 | `1` | `13 * 2 + 1` | `27` |

The linked list represents:

```text
11011₂ = 27₁₀
```
