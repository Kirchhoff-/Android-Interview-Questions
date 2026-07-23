# Floyd's Cycle Detection Algorithm (Tortoise and Hare Algorithm)

Floyd's Cycle Detection Algorithm, also known as the **Tortoise and Hare Algorithm**, uses two pointers moving at different speeds to determine whether a linked list contains a cycle.

The slower pointer, called the **tortoise**, moves one node at a time, while the faster pointer, called the **hare**, moves two nodes at a time.

If the linked list does not contain a cycle, the hare eventually reaches the end of the list. If a cycle exists, both pointers eventually enter the cycle and meet at the same node.

The main question is:

> **Why are the two pointers guaranteed to meet inside the cycle?**

## Why It Works

Let's first see what happens when a linked list **does not** contain a cycle.

```text
1 → 2 → 3 → 4 → 5 → null
```

The tortoise moves one node at a time, while the hare moves two nodes at a time.

Eventually, the hare reaches the end of the list (`null`), and the algorithm stops. Since the hare can leave the list, the two pointers are never guaranteed to meet.

Now consider a linked list that **does** contain a cycle.

```text
 1 → 2 → 3 → 4 → 5
         ↑       ↓
         └───────┘
```

Both pointers eventually enter the cycle. From this point on, neither pointer can leave it because there is no `null` node.

The hare gains **one node** on the tortoise during every iteration. Since the cycle contains a finite number of nodes, the hare cannot keep gaining on the tortoise forever. Eventually, the distance between them becomes zero, meaning both pointers point to the same node.


For example:

```text
Initial state

 T, H
 ↓
 1 → 2 → 3 → 4 → 5
         ↑       ↓
         └───────┘
```

```text
Step 1

    T   H
    ↓   ↓
1 → 2 → 3 → 4 → 5
        ↑       ↓
        └───────┘
```

The tortoise moves from `1` to `2`, while the hare moves from `1` to `3`.

```text
Step 2

        T       H
        ↓       ↓
1 → 2 → 3 → 4 → 5
        ↑       ↓
        └───────┘
```

The tortoise moves from `2` to `3`, while the hare moves from `3` to `5`.

```text
Step 3

            T, H
             ↓
1 → 2 → 3 → 4 → 5
        ↑       ↓
        └───────┘
```

The tortoise moves from `3` to `4`. The hare moves from `5` to `3`, and then from `3` to `4`.

Both pointers now point to the same node, so the cycle has been detected. Once the pointers meet, we know that the linked list contains a cycle.

## Implementation and Complexity

Let's assume we have the following "standard" singly linked list structure:

```kotlin
class ListNode(
    val value: Int,
    var next: ListNode? = null,
)
```

The implementation is surprisingly simple and follows the idea we discussed in the previous section. We create two pointers that initially point to the first node:

- `slow` moves one node at a time;
- `fast` moves two nodes at a time.

As long as fast and fast.next are not null, the hare can safely move two nodes forward, while the tortoise continues moving one node at a time.

```kotlin
fun hasCycle(head: ListNode?): Boolean {
    var slow = head
    var fast = head

    while (fast != null && fast.next != null) {
        slow = slow?.next
        fast = fast.next.next

        if (slow === fast) {
            return true
        }
    }

    return false
}
```

We use referential equality (`===`) because we need to check whether both pointers refer to the **same node**, not whether two different nodes contain the same value.

If the list contains a cycle, `slow` and `fast` eventually meet, and the method returns `true`.

If there is no cycle, `fast` eventually reaches `null`, the loop stops, and the method returns `false`.

**Complexity**

**Time Complexity:** `O(n)`

In the worst case, the pointers traverse the linked list only once before either meeting inside the cycle or reaching the end of the list.

**Space Complexity:** `O(1)`

The algorithm uses only two pointers and does not require any additional data structures.

## Where Is It Used?

Floyd's Cycle Detection Algorithm is commonly used to solve problems involving cycles.

Some common applications include:
- **Detecting cycles** in linked lists.
- **Finding the entry point of a cycle** after a cycle has been detected.

Both solutions rely on the same fundamental idea: using two pointers moving at different speeds to detect a cycle.

# Further Reading
- [Cycle Detection (Wikipedia)](https://en.wikipedia.org/wiki/Cycle_detection)
- [Floyd's Tortoise and Hare Algorithm (CP-Algorithms)](https://cp-algorithms.com/others/tortoise_and_hare.html)
- [Finding a Loop in a Linked List (GeeksforGeeks)](https://www.geeksforgeeks.org/dsa/floyds-cycle-finding-algorithm/)
- [Floyd, R. W. — Non-deterministic Algorithms (Original Paper, 1967)](https://dl.acm.org/doi/10.1145/321420.321422)
