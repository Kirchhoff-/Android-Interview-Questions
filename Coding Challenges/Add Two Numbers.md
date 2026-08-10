# Add Two Numbers
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-gold)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given two non-empty linked lists represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Each linked list represents a non-negative integer. Every node contains a single digit, and the digits are stored in reverse order.

Add the two numbers and return their sum as a linked list in the same reverse order.

## Constraints

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= value <= 9`
- The represented numbers do not contain leading zeros, except for the number `0` itself.

## Examples

**Example 1**

```text
Input:

l1:
3 -> 2 -> 1

l2:
6 -> 5 -> 4

Output:

9 -> 7 -> 5

Explanation:
123 + 456 = 579
```

**Example 2**

```text
Input:

l1:
9 -> 9

l2:
1

Output:

0 -> 0 -> 1

Explanation:
99 + 1 = 100
```

**Example 3**

```text
Input:

l1:
5 -> 4 -> 3

l2:
2 -> 1

Output:

7 -> 5 -> 3

Explanation:
345 + 12 = 357
```

## Solution

The main idea is to process both linked lists simultaneously and add their values one by one. Since the digits are stored in reverse order, we can traverse both lists from the beginning, add the corresponding digits, and immediately append the resulting digit to a new linked list.

The main thing we need to handle carefully is several edge cases:

- **Different list lengths.** One linked list may end before the other. In this case, we treat the missing value as `0` and continue processing the remaining nodes.
- **Carry between digits.** If the sum of the current digits and the previous carry is greater than or equal to `10`, we store only the last digit and carry `1` to the next position.
- **Carry after both lists end.** The last addition may produce a carry even when there are no nodes left in either list. In this case, we need to add one more node to the result.

To handle these cases, we keep two pointers for the input lists and a `carry` value. On each iteration, we add the values of the current nodes together with `carry`. If one of the lists has already ended, its value is treated as `0`.

The last digit of the sum becomes the value of the new node, while the carry is saved for the next iteration. We continue until both input lists are fully processed and there is no carry left.

To build the result, we use a `dummy` node and keep a separate pointer to the last added node. This allows us to append new nodes without handling the first node as a special case.

```kotlin
fun addTwoNumbers(l1: ListNode?, l2: ListNode?): ListNode? {
    val dummy = ListNode(0)

    var l1Pointer = l1
    var l2Pointer = l2
    var resultPointer: ListNode? = dummy

    var carry = 0

    while (l1Pointer != null || l2Pointer != null || carry != 0) {
        val currentNumber = (l1Pointer?.`val` ?: 0) + (l2Pointer?.`val` ?: 0) + carry
        carry = if (currentNumber >= 10) 1 else 0

        val newNode = ListNode(currentNumber % 10)
        resultPointer?.next = newNode

        resultPointer = resultPointer?.next
        l1Pointer = l1Pointer?.next
        l2Pointer = l2Pointer?.next
    }

    return dummy.next
}
```

### Complexity

* Time complexity: `O(max(n, m))`

We process each node from both linked lists once, where `n` and `m` are the lengths of the two lists.

* Space complexity: `O(max(n, m))`

We create a new linked list for the result. It contains at most `max(n, m) + 1` nodes if an additional node is required for the final `carry`.

### Step-by-step

#### Example 1

```text
l1:
3 -> 2 -> 1

l2:
6 -> 5
```

These lists represent `123` and `56`.

| Step | l1 value | l2 value | carry | currentNumber | New node | New carry | Result |
|------|----|----|-------|---------------|----------|-----------|--------|
| 1 | 3 | 6 | 0 | 9 | 9 | 0 | 9 |
| 2 | 2 | 5 | 0 | 7 | 7 | 0 | 9 -> 7 |
| 3 | 1 | 0 | 0 | 1 | 1 | 0 | 9 -> 7 -> 1 |

The second list ends after the second step, so we use `0` as its value during the final iteration.

The resulting list is:

```text
9 -> 7 -> 1
```

which represents `179`.

---

#### Example 2

```text
l1:
9 -> 9

l2:
1
```

These lists represent `99` and `1`.

| Step | l1 value | l2 value  | carry | currentNumber | New node | New carry | Result |
|------|----|----|-------|---------------|----------|-----------|--------|
| 1 | 9 | 1 | 0 | 10 | 0 | 1 | 0 |
| 2 | 9 | 0 | 1 | 10 | 0 | 1 | 0 -> 0 |
| 3 | 0 | 0 | 1 | 1 | 1 | 0 | 0 -> 0 -> 1 |

After the second step, both lists are already fully processed, but `carry` is still equal to `1`. This causes one additional iteration and creates the final node.

The resulting list is:

```text
0 -> 0 -> 1
```

which represents `100`.
