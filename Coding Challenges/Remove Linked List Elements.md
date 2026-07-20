# Remove Linked List Elements
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the `head` of a linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Remove every node whose `value` equals the given `target` value and return the head of the modified linked list.

## Constraints

- The number of nodes in the list is in the range `[0, 10⁴]`.
- `1 <= value <= 50`
- `0 <= target <= 50`

## Examples

```text
Input:
target = 3

1 -> 2 -> 3 -> 4 -> 5

Output:

1 -> 2 -> 4 -> 5
```

```text
Input:
target = 7

7 -> 7 -> 7 -> 7

Output:

[]
```

```text
Input:
target = 2

2 -> 1 -> 2 -> 3 -> 2

Output:

1 -> 3
```

## Solution

This challenge continues the linked list topic introduced in [Remove Duplicates from Sorted List](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Remove%20duplicates%20from%20Sorted%20List.md). The core idea is almost the same: we iterate through the list, check the next node, and skip it by changing the `next` reference when it should be removed.

In the previous challenge, we could safely start from the first node because the sorted list guaranteed that at least one instance of every value had to remain. Even if several duplicate nodes appeared at the beginning, the first one was always unique in the resulting list.

That assumption does not work here. For example:

```text
7 -> 7 -> 7 -> 7
```

If `target = 7`, every node must be removed, including the original `head`. The previous approach only removes nodes that come after `current`, so it cannot remove the first node because there is no node before it whose `next` reference we can update.

To avoid handling the head as a separate special case, we add a temporary node before it:

```text
new node -> 7 -> 7 -> 7 -> 7
```

This node is usually called a `dummy` node. It does not belong to the original list and its value does not matter. Its only purpose is to ensure that every real node, including the original head, has a node before it.

We keep `dummy` at the beginning and use `current` to iterate through the list. If `current.next` contains the target value, we remove that node by making `current.next` point to the node after it.

```text
Before:

current
   ↓
dummy -> 7 -> 1 -> 2
         ↑
      remove

After:

current
   ↓
dummy ------> 1 -> 2
```

After removing a node, we do not move `current`. The new `current.next` may also contain the target value and must be checked during the next iteration.

If the next node should remain, we move `current` forward and continue the traversal.

```kotlin
fun removeElements(head: ListNode?, target: Int): ListNode? {
    val dummy = ListNode(0).apply {
        next = head
    }
    var current: ListNode? = dummy

    while (current?.next != null) {
        if (current.next?.value == target) {
            current.next = current.next?.next
        } else {
            current = current.next
        }
    }

    return dummy.next
}
```

We return `dummy.next` instead of the original `head` because the original first node may have been removed.

### Complexity

* Time complexity: `O(n)`

  We traverse the linked list once and process every node at most one time.

* Space complexity: `O(1)`

  We only create one additional dummy node and use a constant number of references.

### Step-by-step

#### Example 1

```text
target = 2

2 -> 1 -> 2 -> 3 -> 2
```

First, we add the dummy node:

```text
dummy -> 2 -> 1 -> 2 -> 3 -> 2
^
current
```

The algorithm always checks `current.next`, not `current` itself.

**Step 1**

```text
dummy -> 2 -> 1 -> 2 -> 3 -> 2
^        ^
current  current.next
```

`current.next.value` equals `target`, so we skip this node:

```text
current.next = current.next.next
```

```text
dummy -> 1 -> 2 -> 3 -> 2
^
current
```

`current` does not move because its new next node must also be checked.

**Step 2**

```text
dummy -> 1 -> 2 -> 3 -> 2
^        ^
current  current.next
```

The next value is not equal to `target`, so we keep it and move `current` forward:

```text
dummy -> 1 -> 2 -> 3 -> 2
         ^
      current
```

**Step 3**

```text
dummy -> 1 -> 2 -> 3 -> 2
         ^    ^
      current current.next
```

The next value equals `target`, so we skip it:

```text
dummy -> 1 ------> 3 -> 2
         ^
      current
```

After updating the link:

```text
dummy -> 1 -> 3 -> 2
         ^
      current
```

**Step 4**

```text
dummy -> 1 -> 3 -> 2
         ^    ^
      current current.next
```

The next value is not equal to `target`, so we move `current` forward:

```text
dummy -> 1 -> 3 -> 2
              ^
           current
```

**Step 5**

```text
dummy -> 1 -> 3 -> 2
              ^    ^
           current current.next
```

The next value equals `target`, so we skip it:

```text
dummy -> 1 -> 3
              ^
           current
```

There is no next node, so the traversal is complete.

The result starts from `dummy.next`:

```text
1 -> 3
```

---

#### Example 2

```text
target = 7

7 -> 7 -> 7
```

First, we add the dummy node:

```text
dummy -> 7 -> 7 -> 7
^
current
```

The algorithm always checks `current.next`, not `current` itself.

**Step 1**

```text
dummy -> 7 -> 7 -> 7
^        ^
current  current.next
```

The next value equals `target`, so we skip this node:

```text
dummy -> 7 -> 7
^
current
```

`current` does not move because its new next node must also be checked.

**Step 2**

```text
dummy -> 7 -> 7
^        ^
current  current.next
```

The next value also equals `target`, so we skip it again:

```text
dummy -> 7
^
current
```

**Step 3**

```text
dummy -> 7
^        ^
current  current.next
```

The final node also matches `target`, so we skip it:

```text
dummy
^
current
```

There is no next node, so the traversal is complete.

The result starts from `dummy.next`, which is `null`:

```text
[]
```
