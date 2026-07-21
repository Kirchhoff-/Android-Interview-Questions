# Reverse Linked List
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

## Description

You are given the head of a singly linked list represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Reverse the linked list and return the head of the reversed list.

Do not create new nodes. Reverse the list by updating the existing `next` references.

## Constraints

- The number of nodes in the list is in the range `[0, 5000]`.
- `-5000 <= value <= 5000`

## Examples

```text
Input:

1 -> 2 -> 3 -> 4

Output:

4 -> 3 -> 2 -> 1
```

```text
Input:

7 -> 8

Output:

8 -> 7
```

```text
Input:

[]

Output:

[]
```

## Solution

`Reverse Linked List` is one of the fundamental challenges in the linked list topic. Many other problems rely on the same idea of changing the direction of `next` references, so understanding this solution makes more advanced linked list challenges much easier.

The original list looks like this:

```text
1 -> 2 -> 3 -> null
```

To reverse it, every node must point to the node that previously came before it:

```text
null <- 1 <- 2 <- 3
```

The main difficulty is that changing a node's `next` reference can disconnect us from the remaining part of the list. For example, if `current` points to `1` and we immediately change `current.next`, we lose the reference to `2 -> 3`.

Therefore, before reversing the current reference, we must save the next node:

```kotlin
next = current.next
```

The algorithm uses three references:

- `previous` points to the already reversed part of the list.
- `current` points to the node we are processing now.
- `next` temporarily stores the beginning of the unprocessed part.

At the beginning, no nodes have been reversed yet, so `previous` is `null`. The `current` reference points to the original head:

```text
previous: null

current: 1 -> 2 -> 3 -> null
```

During each iteration, we first save the next node:

```kotlin
next = current.next
```

Then we reverse the current reference:

```kotlin
current.next = previous
```

After that, the current node becomes the beginning of the reversed part:

```kotlin
previous = current
```

Finally, we move to the next unprocessed node:

```kotlin
current = next
```

Each iteration moves one node from the unprocessed part to the beginning of the reversed part:

```text
Reversed:      null
Unprocessed:   1 -> 2 -> 3

Reversed:      1
Unprocessed:   2 -> 3

Reversed:      2 -> 1
Unprocessed:   3

Reversed:      3 -> 2 -> 1
Unprocessed:   null
```

When `current` becomes `null`, all nodes have been processed. At this point, `previous` points to the new head of the reversed list.

```kotlin
fun reverseList(head: ListNode?): ListNode? {
    var previous: ListNode? = null
    var next: ListNode? = null
    var current: ListNode? = head

    while (current != null) {
        next = current.next

        current.next = previous
        previous = current
        current = next
    }

    return previous
}
```

### Complexity

* Time complexity: `O(n)`

We process every node exactly once, where `n` is the number of nodes in the list.

* Space complexity: `O(1)`

We use only three additional references and reverse the existing nodes without creating a new list.

### Step-by-step

```text
Input:

1 -> 2 -> 3 -> 4
```

**Initial**

```text
previous = null
current = 1
next = null
```

**Step 1**

Before:

```text
previous = null
current = 1
next = null
```

We save `current.next` in `next`, make `1.next` point to `previous`, move `previous` to `1`, and set `current` to `next`.

After:

```text
previous = 1
current = 2
next = 2
```

**Step 2**

Before:

```text
previous = 1
current = 2
next = 2
```

We save `current.next` in `next`, make `2.next` point to `previous`, move `previous` to `2`, and set `current` to `next`.

After:

```text
previous = 2
current = 3
next = 3
```

**Step 3**

Before:

```text
previous = 2
current = 3
next = 3
```

We save `current.next` in `next`, make `3.next` point to `previous`, move `previous` to `3`, and set `current` to `next`.

After:

```text
previous = 3
current = 4
next = 4
```

**Step 4**

Before:

```text
previous = 3
current = 4
next = 4
```

We save `current.next` in `next`, make `4.next` point to `previous`, move `previous` to `4`, and set `current` to `next`.

After:

```text
previous = 4
current = null
next = null
```

The loop stops because `current` is `null`. The new head of the reversed list is stored in `previous`.

```text
Output:

4 -> 3 -> 2 -> 1
```
