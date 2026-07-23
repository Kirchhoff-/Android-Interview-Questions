# Linked List Cycle
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the `head` of a singly linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Determine whether the linked list contains a cycle.

A cycle exists if, by continuously following the `next` reference, it is possible to return to a node that has already been visited. In other words, some node points back to an earlier node in the list instead of eventually reaching `null`.

Return `true` if the linked list contains a cycle. Otherwise, return `false`.

## Constraints

- The number of nodes in the list is in the range `[0, 10⁴]`.
- `-10⁵ <= value <= 10⁵`

## Examples

**Example 1**

```text
Input:

1 -> 2 -> 3 -> 4
          ↑    |
          └────┘

Output:

true
```

**Example 2**

```text
Input:

1 -> 2
↑    |
└────┘

Output:

true
```

**Example 3**

```text
Input:

1 -> 2 -> 3 -> 4 -> null

Output:

false
```

## Solution

The main idea behind this solution is explained in detail in [Floyd's Cycle Detection Algorithm (Tortoise and Hare Algorithm)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Fundamentals/Floyd's%20Cycle%20Detection%20Algorithm%20(Tortoise%20and%20Hare%20Algorithm).md).

We use two pointers that move through the linked list at different speeds:

- `slow` moves one node at a time;
- `fast` moves two nodes at a time.

If the list does not contain a cycle, `fast` eventually reaches `null`.

If a cycle exists, both pointers eventually enter it. Since `fast` moves one node more than `slow` during every iteration, they eventually point to the same node.

```kotlin
fun hasCycle(head: ListNode?): Boolean {
    var slow = head
    var fast = head

    while (fast != null && fast.next != null) {
        slow = slow?.next
        fast = fast.next?.next

        if (slow === fast) {
            return true
        }
    }

    return false
}
```

We use referential equality (`===`) because we need to check whether both pointers refer to the same node, not whether two different nodes contain the same value.

### Complexity

**Time Complexity:** `O(n)`

In the worst case, both pointers traverse only a limited number of nodes before either meeting inside the cycle or reaching the end of the list.

**Space Complexity:** `O(1)`

The algorithm uses only two pointers and does not require any additional data structures.

## Step-by-step

### Example 1

```text
1 -> 2 -> 3 -> 4 -> 5
          ↑         |
          └─────────┘
```

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 1 | 1 | Both pointers start from the head. |
| 1 | 2 | 3 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 3 | 5 | `fast` enters the cycle. |
| 3 | 4 | 4 | Both pointers meet. **Cycle detected.** |

---

### Example 2

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8
               ↑                   |
               └───────────────────┘
```

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 1 | 1 | Both pointers start from the head. |
| 1 | 2 | 3 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 3 | 5 | `slow` moves to `3`, while `fast` moves to `5`. |
| 3 | 4 | 7 | `slow` enters the cycle. Both pointers are now inside it. |
| 4 | 5 | 4 | `fast` passes `slow` and continues moving through the cycle. |
| 5 | 6 | 6 | Both pointers meet. **Cycle detected.** |

---

### Example 3

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> null
```

| Iteration | Slow | Fast | Explanation |
|-----------|:----:|:----:|-------------|
| Initial | 1 | 1 | Both pointers start from the head. |
| 1 | 2 | 3 | `slow` moves one node, while `fast` moves two nodes. |
| 2 | 3 | 5 | `slow` moves to `3`, while `fast` moves to `5`. |
| 3 | 4 | 7 | `fast.next` is `null`, so the loop stops. **No cycle.** |
