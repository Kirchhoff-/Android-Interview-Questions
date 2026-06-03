# Remove duplicates from Sorted List
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the head of a linked list. Remove all duplicate values so that each value appears only once and return the resulting linked list. The linked list is guaranteed to be sorted in ascending order.

## Examples
```text
Input:  1 -> 1 -> 2
Output: 1 -> 2
```

```text
Input:  1 -> 1 -> 2 -> 3 -> 3
Output: 1 -> 2 -> 3
```

## Linked List Structure

The following structure representing a singly linked list node is provided:

```kotlin
class ListNode(var value: Int) {
  var next: ListNode? = null
}
```

## Solution

This is one of the most fundamental linked list problems and a great introduction to pointer manipulation. Many more advanced linked list problems build upon the same ideas used in this challenge.

Since the list is sorted in ascending order, duplicate values can only appear next to each other.

This means we do not need any additional data structures. We can simply iterate through the list and compare each node with the next one.

If both nodes contain the same value, we can remove the duplicate node by updating the `next` reference. Otherwise, we continue moving through the list.

```kotlin
fun deleteDuplicates(head: ListNode?): ListNode? {
    var current = head

    while (current?.next != null) {
        if (current.value == current.next?.value) {
            current.next = current.next?.next
        } else {
            current = current.next
        }
    }

    return head
}
```

### How it works

For the following list:

```text
1 -> 1 -> 2 -> 3 -> 3
```

We start from the first node and compare it with the next one:

```text
1 -> 1 -> 2 -> 3 -> 3
^    ^
```

Since both values are equal, we skip the second node:

```text
1 ------> 2 -> 3 -> 3
```

We then continue through the list:

```text
1 -> 2 -> 3 -> 3
          ^    ^
```

Again, both values are equal, so we skip the duplicate node:

```text
1 -> 2 -> 3
```

### Complexity

* Time complexity: `O(n)`
* Space complexity: `O(1)`