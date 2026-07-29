# Remove Nth node from end of list
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the `head` of a singly linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Remove the `n`th node from the end of the linked list. Return the `head` of the updated list.

## Constraints

- The number of nodes in the list is in the range `[1, 30]`.
- `0 <= Node.value <= 100`.
- `1 <= n <= list.length`

## Examples

**Example 1**

```text
Input:

7 -> 3 -> 9 -> 1 -> 6

n = 3

Output:

7 -> 3 -> 1 -> 6
```

Explanation:

The `3`rd node from the end is `9`, so we remove it from the list.

**Example 2**

```text
Input:

5

n = 1

Output:

null
```

Explanation:

The list contains only one node. After removing it, the linked list becomes empty.

**Example 3**

```text
Input:

4 -> 7 -> 1 -> 9

n = 4

Output:

7 -> 1 -> 9
```

Explanation:

The `4`th node from the end is the head (`4`), so after removing it, `7` becomes the new head.

## Solution 1. Two Passes

The most straightforward approach is to first determine the length of the linked list.

Once we know the total number of nodes, we can convert the position from the end into a position from the beginning. For example, if the list contains `5` nodes and `n = 2`, we need to remove the node at index `3`.

After that, we traverse the list again, stop at the node before the one that should be removed, and update its `next` reference.

```kotlin
fun removeNthFromEnd(head: ListNode?, n: Int): ListNode? {
    var length = 0
    var current = head

    while (current != null) {
        length++
        current = current.next
    }

    val steps = length - n - 1

    current = head
    repeat(steps) {
        current = current?.next
    }

    current?.next = current.next?.next

    return head
}
```

This solution works when the node we need to remove has a previous node. However, it does not cover one important edge case: removing the first node. For example:

```text
1 -> 2 -> 3 -> 4

n = 4
```

The list contains `4` nodes, so:

```text
steps = 4 - 4 - 1 = -1
```

There is no node before the head, so we cannot remove it by updating the `next` reference of a previous node.

To handle this case, we check whether `n` is equal to the length of the list. If it is, the head itself should be removed, so we return `head.next`.

```kotlin
fun removeNthFromEnd(head: ListNode?, n: Int): ListNode? {
    var length = 0
    var current = head

    while (current != null) {
        length++
        current = current.next
    }

    if (n == length) {
        return head?.next
    }

    val steps = length - n - 1

    current = head
    repeat(steps) {
        current = current?.next
    }

    current?.next = current.next?.next

    return head
}
```

## Complexity

**Time Complexity:** `O(n)`

We traverse the linked list twice: once to calculate its length and once to find the node before the one that should be removed.

**Space Complexity:** `O(1)`

We only use a few additional variables, regardless of the size of the linked list.

## Solution 2. Fast and Slow Pointers

The previous solution required traversing the linked list twice: once to calculate its length and once to remove the target node. However, we can solve the same problem in a single traversal by using the **Fast and Slow Pointers** technique.

The general idea is to keep a fixed distance of `n` nodes between two pointers. We first move the `fast` pointer `n` nodes ahead, while the `slow` pointer stays at the beginning of the list.

Once the required distance is established, we move both pointers one node at a time. Since they always move together, the distance between them never changes.

As a result, when `fast` reaches the last node, `slow` will be positioned immediately before the node that should be removed. This allows us to remove the target node by updating a single `next` reference.

There is one small complication. If the first node needs to be removed, there is no previous node to update.

To solve this, we introduce a **dummy** node before the original `head`. This way, every node in the list, including the original head, always has a previous node, so the removal logic remains exactly the same for every case.

```kotlin
fun removeNthFromEnd(head: ListNode?, n: Int): ListNode? {
    val dummy = ListNode(0).apply {
        next = head
    }

    var slow: ListNode? = dummy
    var fast: ListNode? = dummy

    var steps = n
    while (steps > 0) {
        fast = fast?.next
        steps--
    }

    while (fast?.next != null) {
        fast = fast.next
        slow = slow?.next
    }

    slow?.next = slow.next?.next

    return dummy.next
}
```

## Complexity

**Time Complexity:** `O(n)`

We traverse the linked list only once.

**Space Complexity:** `O(1)`

We only use a few additional pointers, regardless of the size of the linked list.

## Step-by-step

### Example 1

```text
7 -> 3 -> 9 -> 1 -> 6

n = 3
```

We add a dummy node before the original `head` and place both pointers on it:

```text
dummy -> 7 -> 3 -> 9 -> 1 -> 6
  ↑
slow
fast
```

First, we move `fast` by `n = 3` nodes:

| Step | `slow` | `fast` | Action |
|---:|:---:|:---:|---|
| Initial | `dummy` | `dummy` | Both pointers start at the dummy node. |
| 1 | `dummy` | `7` | Move `fast` one node forward. |
| 2 | `dummy` | `3` | Move `fast` one node forward. |
| 3 | `dummy` | `9` | Move `fast` one node forward. The distance between the pointers is now `3` nodes. |

Now we move both pointers until `fast` reaches the last node:

| Step | `slow` | `fast` | Action |
|---:|:---:|:---:|---|
| Initial | `dummy` | `9` | `fast.next` is not `null`, so both pointers can move. |
| 1 | `7` | `1` | Move both pointers one node forward. |
| 2 | `3` | `6` | Move both pointers one node forward. `fast` has reached the last node. |

`slow` is now positioned immediately before `9`, which is the `3`rd node from the end.

We remove it by making `slow.next` point to `1` instead of `9`:

```text
7 -> 3 -> 1 -> 6
```

### Example 2

```text
5

n = 1
```

We add a dummy node before the original `head`:

```text
dummy -> 5
  ↑
slow
fast
```

First, we move `fast` by `n = 1` node:

| Step | `slow` | `fast` | Action |
|---:|:---:|:---:|---|
| Initial | `dummy` | `dummy` | Both pointers start at the dummy node. |
| 1 | `dummy` | `5` | Move `fast` one node forward. |

`fast` is already at the last node, so the second loop does not run. `slow` remains on the dummy node, immediately before the node that should be removed.

We remove `5` by changing `dummy.next` to `null`:

```text
null
```

The method returns `dummy.next`, which is now `null`.
