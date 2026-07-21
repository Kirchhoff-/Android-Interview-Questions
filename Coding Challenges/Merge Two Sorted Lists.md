# Merge Two Sorted Lists
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the heads of two sorted linked lists represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Merge both lists into a single sorted linked list and return its head.

The resulting list must contain all nodes from both input lists in non-decreasing order. Reuse the existing nodes by updating their `next` references instead of creating new ones.

## Constraints

- The number of nodes in each list is in the range `[0, 50]`.
- `-100 <= value <= 100`
- Both linked lists are sorted in non-decreasing order.

## Examples

```text
Input:

list1:
1 -> 4 -> 7

list2:
2 -> 3 -> 6

Output:

1 -> 2 -> 3 -> 4 -> 6 -> 7
```

```text
Input:

list1:
5 -> 8

list2:
1 -> 2 -> 3 -> 4

Output:

1 -> 2 -> 3 -> 4 -> 5 -> 8
```

```text
Input:

list1:
[]

list2:
3 -> 5 -> 9

Output:

3 -> 5 -> 9
```

## Solution

This challenge continues the linked list topic from [Remove Linked List Elements](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Remove%20Linked%20List%20Elements.md). The main idea is similar: we reuse the existing nodes and build the result by changing their `next` references.

As in the previous challenge, we use a `dummy` node. It stays before the beginning of the merged list and gives us a stable reference to its head. Without it, we would need to handle the first added node as a separate case.

```text
dummy -> first node -> second node -> ...
```

However, this task requires more references than the previous one:

- `pointerList1` points to the current node of the first list.
- `pointerList2` points to the current node of the second list.
- `tail` points to the last node already added to the merged list.
- `dummy` always stays at the beginning and does not move.

At the beginning, `dummy` and `tail` point to the same temporary node:

```text
dummy
  ↓
temporary node
  ↑
 tail
```

On every iteration, we compare the current nodes from both lists. The node with the smaller `number` is connected to `tail.next`, and only the pointer belonging to that list moves forward.

After that, `tail` moves to the node we have just added:

```text
Before:

dummy -> 1
         ↑
        tail

Next node: 2
```

```text
After:

dummy -> 1 -> 2
              ↑
             tail
```

The algorithm continues until both input lists are fully processed. If one list becomes empty first, the remaining nodes from the other list are added one by one.

```kotlin
fun mergeTwoLists(list1: ListNode?, list2: ListNode?): ListNode? {
    val dummy: ListNode = ListNode(Int.MIN_VALUE)
    var tail: ListNode? = dummy
    var pointerList1: ListNode? = list1
    var pointerList2: ListNode? = list2

    while (pointerList1 != null || pointerList2 != null) {
        when {
            pointerList1 == null -> {
                tail?.next = pointerList2
                pointerList2 = pointerList2?.next
            }

            pointerList2 == null -> {
                tail?.next = pointerList1
                pointerList1 = pointerList1.next
            }

            pointerList1.value < pointerList2.value -> {
                tail?.next = pointerList1
                pointerList1 = pointerList1.next
            }

            else -> {
                tail?.next = pointerList2
                pointerList2 = pointerList2.next
            }
        }

        tail = tail?.next
    }

    return dummy.next
}
```

We return `dummy.next` because `dummy` itself is only a temporary node and must not be included in the result.

### Complexity

* Time complexity: `O(n + m)`

We process every node from both lists exactly once, where `n` and `m` are the numbers of nodes in the input lists.

* Space complexity: `O(1)`

We create only one additional dummy node and use a constant number of references. All other nodes are reused from the input lists.

### Improved version

The solution works correctly, but it can be simplified once one of the lists ends.

Both input lists are already sorted. Therefore, if one list is exhausted, all remaining nodes of the other list can be connected to `tail.next` at once:

```text
Merged part:

dummy -> 1 -> 2 -> 3
                  ↑
                 tail

Remaining part:

4 -> 6 -> 7
```

We do not need to add `4`, `6`, and `7` separately because they are already connected and sorted:

```text
dummy -> 1 -> 2 -> 3 -> 4 -> 6 -> 7
```

Because the loop now runs only while both pointers are not `null`, the separate branches for finished lists are no longer needed. Only two possibilities remain inside the loop, so a regular `if`/`else` is simpler than `when`.

```kotlin
fun mergeTwoLists(list1: ListNode?, list2: ListNode?): ListNode? {
    val dummy: ListNode = ListNode(Int.MIN_VALUE)
    var tail: ListNode? = dummy
    var pointerList1: ListNode? = list1
    var pointerList2: ListNode? = list2

    while (pointerList1 != null && pointerList2 != null) {
        if (pointerList1.value < pointerList2.value) {
            tail?.next = pointerList1
            pointerList1 = pointerList1.next
        } else {
            tail?.next = pointerList2
            pointerList2 = pointerList2.next
        }

        tail = tail?.next
    }

    tail?.next = pointerList1 ?: pointerList2

    return dummy.next
}
```

The complexity remains the same:

* Time complexity: `O(n + m)`
* Space complexity: `O(1)`

The second version is shorter and avoids processing the remaining part of one list node by node.

### Step-by-step

#### Example 1

```text
list1:
1 -> 4 -> 7

list2:
2 -> 3 -> 6
```

First, we create the `dummy` node. Both `dummy` and `tail` point to it, while `pointerList1` and `pointerList2` point to the first nodes of the input lists:

```text
Result:

dummy
  ^
 tail

list1:
1 -> 4 -> 7
^
pointerList1

list2:
2 -> 3 -> 6
^
pointerList2
```

**Step 1**

We compare `1` and `2`.

Since `1 < 2`, we connect the current node from `list1` to `tail.next`:

```text
dummy -> 1
         ^
        tail

list1:
4 -> 7
^
pointerList1

list2:
2 -> 3 -> 6
^
pointerList2
```

`pointerList1` moves to `4`, while `pointerList2` stays at `2`.

**Step 2**

We compare `4` and `2`.

Since `2` is smaller, we add the current node from `list2`:

```text
dummy -> 1 -> 2
              ^
             tail

list1:
4 -> 7
^
pointerList1

list2:
3 -> 6
^
pointerList2
```

`pointerList2` moves to `3`.

**Step 3**

We compare `4` and `3`.

Since `3` is smaller, we add it to the result:

```text
dummy -> 1 -> 2 -> 3
                   ^
                  tail

list1:
4 -> 7
^
pointerList1

list2:
6
^
pointerList2
```

`pointerList2` moves to `6`.

**Step 4**

We compare `4` and `6`.

Since `4 < 6`, we add the current node from `list1`:

```text
dummy -> 1 -> 2 -> 3 -> 4
                        ^
                       tail

list1:
7
^
pointerList1

list2:
6
^
pointerList2
```

`pointerList1` moves to `7`.

**Step 5**

We compare `7` and `6`.

Since `6` is smaller, we add the current node from `list2`:

```text
dummy -> 1 -> 2 -> 3 -> 4 -> 6
                             ^
                            tail

list1:
7
^
pointerList1

list2:
[]
```

`pointerList2` becomes `null`, so the loop stops.

The first list still contains one node:

```text
7
^
pointerList1
```

Because the remaining part is already sorted, we connect it directly to `tail.next`:

```text
dummy -> 1 -> 2 -> 3 -> 4 -> 6 -> 7
```

The result starts from `dummy.next`:

```text
1 -> 2 -> 3 -> 4 -> 6 -> 7
```

---

#### Example 2

```text
list1:
1 -> 2

list2:
3 -> 4 -> 5 -> 6
```

Initially:

```text
Result:

dummy
  ^
 tail

list1:
1 -> 2
^
pointerList1

list2:
3 -> 4 -> 5 -> 6
^
pointerList2
```

**Step 1**

We compare `1` and `3`.

Since `1 < 3`, we add `1` and move `pointerList1` forward:

```text
dummy -> 1
         ^
        tail

list1:
2
^
pointerList1

list2:
3 -> 4 -> 5 -> 6
^
pointerList2
```

**Step 2**

We compare `2` and `3`.

Since `2 < 3`, we add `2`:

```text
dummy -> 1 -> 2
              ^
             tail

list1:
[]

list2:
3 -> 4 -> 5 -> 6
^
pointerList2
```

`pointerList1` becomes `null`, so the loop stops.

We do not need to process `3`, `4`, `5`, and `6` separately. These nodes are already sorted and connected, so we attach the entire remaining part to `tail.next`:

```text
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> 6
```

The result starts from `dummy.next`:

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```
