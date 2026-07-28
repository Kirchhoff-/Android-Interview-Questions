# Intersection of Two Linked Lists
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Linked%20List-blue)

You are given the heads of two singly linked lists represented by the following structure:

```kotlin
class ListNode(var value: Int) {
    var next: ListNode? = null
}
```

Your task is to return the first node where the two linked lists intersect.

Two linked lists intersect only if they share the **same node object**. Having nodes with the same value does **not** mean the lists intersect.

If the two linked lists do not intersect, return `null`.

The linked lists must remain unchanged after the function returns.

## Constraints

- The number of nodes in `listA` is in the range `[1, 3 * 10⁴]`.
- The number of nodes in `listB` is in the range `[1, 3 * 10⁴]`.
- `1 <= value <= 10⁵`

## Examples

**Example 1**

```text
Input:

List A:
4 -> 1 -> 8 -> 4 -> 5

List B:
5 -> 6 -> 1 -> 8 -> 4 -> 5

Output:

8 -> 4 -> 5
```

Explanation:

The linked lists intersect at node `8`. We return the first shared node and the remaining part of the list.

**Example 2**

```text
Input:

List A:
1 -> 9 -> 1 -> 2 -> 4

List B:
3 -> 2 -> 4

Output:

2 -> 4
```

Explanation:

The linked lists intersect at node `2`. We return the first shared node and the remaining part of the list.

**Example 3**

```text
Input:

List A:
2 -> 6 -> 4

List B:
1 -> 5

Output:

null
```

Explanation:

The linked lists do not share any nodes, so there is no intersection.

## Solution 1. Set

This is probably the first solution that comes to mind, and honestly, it is a very solid one.

In a real project, this approach would likely be used most of the time. It is easy to understand, simple to implement, and makes the intention of the code immediately obvious. Unless memory usage is a strict requirement, there is usually no reason to look for a more complicated solution.

The idea is straightforward:

1. Traverse the first linked list and store every node in a `HashSet`.
2. Traverse the second linked list.
3. As soon as we find a node that already exists in the `HashSet`, we have found the intersection.
4. If we reach the end of the second list without finding a shared node, the lists do not intersect.

```kotlin
fun getIntersectionNode(headA: ListNode?, headB: ListNode?): ListNode? {
    val visited = mutableSetOf<ListNode>()

    var current = headA
    while (current != null) {
        visited += current
        current = current.next
    }

    current = headB
    while (current != null) {
        if (current in visited) {
            return current
        }

        current = current.next
    }

    return null
}
```

### Complexity

**Time Complexity:** `O(n + m)`

We traverse both linked lists once. Every `HashSet` lookup and insertion takes `O(1)` on average.

**Space Complexity:** `O(n)`

In the worst case, we store every node from the first linked list in the `HashSet`.

## Solution 2. Align the Starting Points

The first solution uses additional memory to remember every node from the first linked list. However, we can optimize this solution by taking advantage of the structure of linked lists and reducing the extra memory usage to `O(1)`.

The key observation is that **once two linked lists intersect, they share the same tail**. From the first shared node until the end, both lists consist of exactly the same nodes.

For example:

```text
List A:
1 -> 2 -> 3 -> 7 -> 8

List B:
9 ------> 7 -> 8
```

The shared part is:

```text
7 -> 8
```

Everything before node `7` is unique to each list. This means the only difference between the two linked lists is the number of nodes before the shared tail.

Therefore, we can safely skip the extra nodes at the beginning of the longer list. After doing so, both pointers will be the same distance away from the end of the linked lists.

From that point, we simply move both pointers one node at a time. If the linked lists intersect, they will eventually meet at the first shared node. Otherwise, both pointers will eventually become `null`.

The algorithm is straightforward:

1. Traverse both linked lists and calculate their lengths.
2. Move the pointer of the longer list forward by the difference in lengths.
3. Traverse both linked lists simultaneously.
4. Return the first node where both pointers reference the same object.
5. If no such node exists, return `null`.

```kotlin
fun getIntersectionNode(headA: ListNode?, headB: ListNode?): ListNode? {
    var pointerA = headA
    var pointerB = headB

    var sizeOfA = 0
    var sizeOfB = 0

    while (pointerA != null) {
        sizeOfA++
        pointerA = pointerA.next
    }

    while (pointerB != null) {
        sizeOfB++
        pointerB = pointerB.next
    }

    pointerA = headA
    pointerB = headB

    if (sizeOfA > sizeOfB) {
        repeat(sizeOfA - sizeOfB) {
            pointerA = pointerA?.next
        }
    } else {
        repeat(sizeOfB - sizeOfA) {
            pointerB = pointerB?.next
        }
    }

    while (pointerA !== pointerB) {
        pointerA = pointerA?.next
        pointerB = pointerB?.next
    }

    return pointerA
}
```

### Complexity

**Time Complexity:** `O(n + m)`

We traverse both linked lists once to calculate their lengths, and then traverse them again together to find the intersection. Overall, every node is visited at most twice.

**Space Complexity:** `O(1)`

The algorithm uses only a few pointers and counters without any additional data structures.

## Solution 3. Magic

The second solution already satisfies all complexity requirements. It runs in `O(n + m)` time, uses `O(1)` extra space, and is relatively easy to figure out. We can make this solution even shorter, but we have to pay a price for that.

This problem is a good example of why many developers do not enjoy coding challenges. The first and second solutions can be discovered logically, even if you have never seen this problem before.

This third solution is different. It is based on a clever observation that is difficult to come up with during an interview unless you have encountered it before.

This solution achieves exactly the same result, but without calculating the lengths of the linked lists.

The whole idea behind this solution is the following:

> When `pointerA` reaches the end of list A, it continues from the head of list B. Likewise, when `pointerB` reaches the end of list B, it continues from the head of list A. Both pointers keep moving one node at a time until they either reference the same shared node or both become `null`.

At first glance, this algorithm looks almost like magic. We never calculate the lengths of the linked lists, we never align their starting positions, and yet both pointers somehow arrive at exactly the same place.

Naturally, this raises several questions:

- Why does switching the linked lists work?
- Why do both pointers eventually become aligned?
- Why doesn't this algorithm get stuck in an infinite loop?
- Why do both pointers become `null` at the same time when there is no intersection?

To understand why this works, imagine the path that each pointer travels.

```
pointerA:
A + B

pointerB:
B + A
```

Although the pointers traverse the linked lists in a different order, they both travel exactly the same total distance. `pointerA` visits every node from list A and then every node from list B. `pointerB` does the opposite: it visits every node from list B and then every node from list A.

This means that both pointers traverse exactly the same total number of nodes. If one pointer spends more time in the longer unique part of its linked list, the other pointer spends the same extra time there after switching to the opposite linked list. As a result, the difference in the lengths of the unique prefixes disappears automatically, without ever calculating their lengths.

Once both pointers enter the shared part of the linked lists, they are perfectly aligned and continue moving together. The first time they reference the same node, we have found the intersection.

If the linked lists do not intersect, there is no shared part to align on. In that case, both pointers simply finish traversing `A + B`, become `null` at the same time, and the loop terminates naturally.

```kotlin
fun getIntersectionNode(headA: ListNode?, headB: ListNode?): ListNode? {
    var pointerA = headA
    var pointerB = headB

    while (pointerA !== pointerB) {
        pointerA = if (pointerA == null) headB else pointerA.next
        pointerB = if (pointerB == null) headA else pointerB.next
    }

    return pointerA
}
```

### Complexity

**Time Complexity:** `O(n + m)`

Each pointer traverses both linked lists at most once.

**Space Complexity:** `O(1)`

The algorithm uses only two pointers.

## Step-by-Step

To make the pointer movements easier to follow, shared nodes are marked with the same names in both linked lists. For example, `C1(8)` means that both lists reference the same node with the value `8`.

### Example 1

```text
List A:
A1(4) -> A2(1) -> C1(8) -> C2(4) -> C3(5)

List B:
B1(5) -> B2(6) -> B3(1) -> C1(8) -> C2(4) -> C3(5)
```

The lists have different lengths, so the pointers initially reach the shared part at different moments. After switching lists, this difference is compensated automatically.

| Step | `pointerA` | `pointerB` | Action |
|---:|---|---|---|
| 0 | `A1(4)` | `B1(5)` | The pointers reference different nodes, so both move forward. |
| 1 | `A2(1)` | `B2(6)` | The pointers still reference different nodes. |
| 2 | `C1(8)` | `B3(1)` | `pointerA` has already reached the shared part, but `pointerB` has not. |
| 3 | `C2(4)` | `C1(8)` | Both pointers are inside the shared part, but they are not aligned yet. |
| 4 | `C3(5)` | `C2(4)` | The pointers continue moving one node at a time. |
| 5 | `null` | `C3(5)` | `pointerA` reaches the end of list A. |
| 6 | `B1(5)` | `null` | `pointerA` switches to list B, while `pointerB` reaches the end of list B. |
| 7 | `B2(6)` | `A1(4)` | Both pointers have switched lists, so the difference between the unique prefixes is now compensated. |
| 8 | `B3(1)` | `A2(1)` | The pointers are now aligned and continue moving toward the shared part. |
| 9 | `C1(8)` | `C1(8)` | Both pointers reference the same node, so the intersection is found. |

The algorithm returns `C1(8)`:

```text
8 -> 4 -> 5
```

### Example 2

```text
List A:
A1(1) -> A2(9) -> A3(1) -> C1(2) -> C2(4)

List B:
B1(3) -> C1(2) -> C2(4)
```

In this example, list A has a much longer unique prefix than list B.

| Step | `pointerA` | `pointerB` | Action |
|---:|---|---|---|
| 0 | `A1(1)` | `B1(3)` | The pointers start at the heads of their linked lists. |
| 1 | `A2(9)` | `C1(2)` | `pointerB` reaches the shared part first. |
| 2 | `A3(1)` | `C2(4)` | The pointers reference different nodes. |
| 3 | `C1(2)` | `null` | `pointerA` reaches the shared part, while `pointerB` reaches the end of list B. |
| 4 | `C2(4)` | `A1(1)` | `pointerB` switches to the head of list A. |
| 5 | `null` | `A2(9)` | `pointerA` reaches the end of list A. |
| 6 | `B1(3)` | `A3(1)` | `pointerA` switches to the head of list B. Both pointers have switched lists, so the difference between the unique prefixes is now compensated. |
| 7 | `C1(2)` | `C1(2)` | Both pointers reference the same shared node. |

The algorithm returns `C1(2)`:

```text
2 -> 4
```

### Example 3

```text
List A:
A1(2) -> A2(6) -> A3(4)

List B:
B1(1) -> B2(5)
```

These linked lists do not intersect. Each pointer still traverses both lists, but there is no shared node where they can meet.

| Step | `pointerA` | `pointerB` | Action |
|---:|---|---|---|
| 0 | `A1(2)` | `B1(1)` | The pointers start at different nodes. |
| 1 | `A2(6)` | `B2(5)` | Both pointers move forward. |
| 2 | `A3(4)` | `null` | `pointerB` reaches the end of list B. |
| 3 | `null` | `A1(2)` | `pointerB` switches to list A, while `pointerA` reaches the end of list A. |
| 4 | `B1(1)` | `A2(6)` | `pointerA` switches to list B. |
| 5 | `B2(5)` | `A3(4)` | Both pointers continue moving through the opposite lists. |
| 6 | `null` | `null` | Both pointers become `null` at the same time, so there is no intersection. |

The algorithm returns:

```text
null
```
