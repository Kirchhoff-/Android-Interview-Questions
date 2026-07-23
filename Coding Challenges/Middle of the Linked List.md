# Middle of the Linked List
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the `head` of a singly linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Return the middle node of the linked list. If the list contains an odd number of nodes, return the only middle node. If the list contains an even number of nodes, return the second of the two middle nodes.

## Constraints

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= value <= 100`

## Examples

**Example 1**

```text
Input:

4 -> 8 -> 15 -> 16 -> 23

Output:

15 -> 16 -> 23

Explanation:

The list contains five nodes, so node 15 is the only middle node.
```

**Example 2**

```text
Input:

2 -> 6 -> 10 -> 14 -> 18 -> 22 -> 26 -> 30

Output:

18 -> 22 -> 26 -> 30

Explanation:

The list contains eight nodes, so it has two middle nodes with values 14 and 18. We return the second middle node.
```

**Example 3**

```text
Input:

7 -> 11 -> 19

Output:

11 -> 19

Explanation:

The list contains three nodes, so node 11 is the only middle node.
```

## Solution

The main idea behind this solution is explained in detail in [Floyd's Cycle Detection Algorithm (Tortoise and Hare Algorithm)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Fundamentals/Floyd's%20Cycle%20Detection%20Algorithm%20(Tortoise%20and%20Hare%20Algorithm).md).

The idea behind this solution is to create two pointers that move through the linked list at different speeds.

The `slow` pointer moves one node at a time, while the `fast` pointer moves two nodes at a time. Since `fast` is twice as fast, it reaches the end of the list first. By that moment, `slow` has traversed exactly half of the list and therefore points to the middle node.

If the list contains an even number of nodes, `slow` automatically advances to the second middle node before the loop terminates, which is exactly what the problem asks us to return.

```kotlin
fun middleNode(head: ListNode?): ListNode? {
    var slow: ListNode? = head
    var fast: ListNode? = head

    while (fast != null && fast.next != null) {
        slow = slow?.next
        fast = fast.next?.next
    }

    return slow
}
```

### Complexity

**Time Complexity:** `O(n)`

The `fast` pointer traverses the list once, while the `slow` pointer traverses only half of it.

**Space Complexity:** `O(1)`

The algorithm uses only two pointers and does not require any additional data structures.

## Step-by-step

### Example 1

```text
1 -> 2 -> 3 -> 4 -> 5
```

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 1 | 1 | Both pointers start from the head. |
| 1 | 2 | 3 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 3 | 5 | `slow` reaches the middle node, while `fast` reaches the last node. |
| Stop | 3 | 5 | `fast.next` is `null`, so the loop stops. We return the node pointed to by `slow`. |

---

### Example 2

```text
2 -> 6 -> 10 -> 14 -> 18 -> 22 -> 26 -> 30
```

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 2 | 2 | Both pointers start from the head. |
| 1 | 6 | 10 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 10 | 18 | `slow` moves to `10`, while `fast` moves to `18`. |
| 3 | 14 | 26 | `slow` moves to the first middle node, while `fast` moves to `26`. |
| 4 | 18 | `null` | `slow` moves to the second middle node, while `fast` reaches the end of the list. |
| Stop | 18 | `null` | The loop stops, we return the node pointed to by `slow`. |
