# Palindrome Linked List
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the `head` of a singly linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Return `true` if the values in the linked list form a palindrome, or `false` otherwise. A palindrome reads the same from left to right and from right to left.

## Constraints

- The number of nodes in the list is in the range `[1, 10⁵]`.
- `0 <= value <= 9`

## Examples

**Example 1**

```text
Input:

1 -> 2 -> 3 -> 2 -> 1

Output:

true

Explanation:

The values are the same when read from left to right and from right to left.
```

**Example 2**

```text
Input:

4 -> 8 -> 8 -> 4

Output:

true

Explanation:

The first value matches the last value, and the two middle values are also equal.
```

**Example 3**

```text
Input:

2 -> 6 -> 5 -> 6 -> 3

Output:

false

Explanation:

The first and last values are different, so the linked list is not a palindrome.
```

## Solution

This challenge is essentially a combination of two linked list problems:

- [Middle of the Linked List](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Middle%20of%20the%20Linked%20List.md)
- [Reverse Linked List](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Reverse%20Linked%20List.md)

The main idea behind the solution is:

- Find the middle of the linked list;
- Reverse the right half;
- Compare the left and right parts node by node.

After finding the middle, we reverse only the second half of the list. This allows us to traverse the original left half and the reversed right half in the same direction.

For an even-length list, both halves have the same length. For an odd-length list, the middle node does not affect whether the list is a palindrome, so we simply skip it.

Finally, we compare both halves node by node. If we find at least one mismatch, we return `false`. Otherwise, after reaching the end of the right half, we know that every corresponding pair of values is equal, so we return `true`.

The implementation below follows exactly these three steps.

```kotlin
fun isPalindrome(head: ListNode?): Boolean {
    var leftPart = head

    val rightPartStart = middleNode(head)
    var rightPart = reverseList(rightPartStart)

    while (rightPart != null) {
        if (leftPart?.value != rightPart.value) {
            return false
        }

        leftPart = leftPart.next
        rightPart = rightPart.next
    }

    return true
}

private fun middleNode(head: ListNode?): ListNode? {
    var slow = head
    var fast = head

    while (fast != null && fast.next != null) {
        slow = slow?.next
        fast = fast.next?.next
    }

    if (fast == null) {
        // Even-length list.
        return slow
    } else {
        // Odd-length list: skip the middle node.
        return slow?.next
    }
}

private fun reverseList(head: ListNode?): ListNode? {
    var previous: ListNode? = null
    var current = head

    while (current != null) {
        val next = current.next

        current.next = previous
        previous = current
        current = next
    }

    return previous
}
```

### Complexity

**Time Complexity:** `O(n)`

Finding the middle takes `O(n)`, reversing the second half takes `O(n)`, and comparing both halves also takes `O(n)`. Although these operations are performed one after another, their total complexity is still `O(n)`.

**Space Complexity:** `O(1)`

The algorithm uses only a few pointers and reverses the linked list in place without allocating any additional data structures.

## Step-by-step

### Example 1

```text
1 -> 2 -> 3 -> 2 -> 1
```

First, we find the beginning of the right part using the `slow` and `fast` pointers.

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 1 | 1 | Both pointers start from the head. |
| 1 | 2 | 3 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 3 | 1 | `slow` reaches the middle node, while `fast` reaches the last node. |
| Stop | 3 | 1 | `fast.next` is `null`, so the loop stops. Since the list length is odd, we skip the middle node and start the right part from `slow.next`. |

The right part is:

```text
2 -> 1
```

After reversing it:

```text
1 -> 2
```

Now we compare the left part with the reversed right part.

| Step | Left Part | Right Part | Result |
|------|:---------:|:----------:|--------|
| 1 | 1 | 1 | The values are equal, so we continue. |
| 2 | 2 | 2 | The values are equal, so we continue. |
| Stop | 3 | `null` | All nodes from the right part have been checked, so we return `true`. |

---

### Example 2

```text
4 -> 8 -> 6 -> 4
```

First, we find the beginning of the right part.

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 4 | 4 | Both pointers start from the head. |
| 1 | 8 | 6 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 6 | `null` | `slow` reaches the beginning of the right part, while `fast` reaches the end of the list. |
| Stop | 6 | `null` | Since the list length is even, the right part starts directly from `slow`. |

The right part is:

```text
6 -> 4
```

After reversing it:

```text
4 -> 6
```

Now we compare both parts.

| Step | Left Part | Right Part | Result |
|------|:---------:|:----------:|--------|
| 1 | 4 | 4 | The values are equal, so we continue. |
| 2 | 8 | 6 | The values are different, so we return `false`. |
