# Maximum Depth of Binary Tree

![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Binary%20Tree-blue)

Given the `root` of a binary tree, return its maximum depth.

The maximum depth of a binary tree is the number of nodes along the longest path from the root node down to the farthest leaf node.

## Examples

**Example 1:**

```text
       8
      / \
     3   12
        /  \
       9   14
```

```kotlin
Input: root = [8,3,12,null,null,9,14]
Output: 3
```

Explanation:

```text
Longest paths:
8 -> 12 -> 9
8 -> 12 -> 14

Number of nodes = 3
```

---

**Example 2:**

```text
    5
     \
      8
```

```kotlin
Input: root = [5,null,8]
Output: 2
```

Explanation:

```text
Longest path:
5 -> 8

Number of nodes = 2
```

## Constraints

```kotlin
0 <= Number of Nodes <= 10_000

-100 <= Node Value <= 100
```

## Structure

The following structure representing a binary tree node is provided:

```kotlin
class TreeNode(var value: Int) {

    var left: TreeNode? = null

    var right: TreeNode? = null
}
```

Each node contains:

- `value` - value stored in the node.
- `left` - reference to the left child.
- `right` - reference to the right child.

## Solution

This problem is a classic **Depth-First Search (DFS)** problem. The key observation is that the depth of a node depends on the depth of its children.

For every node:

- Calculate the depth of the left subtree.
- Calculate the depth of the right subtree.
- Take the larger value.
- Add `1` for the current node.

The recursion stops when we reach a `null` node. A `null` node has a depth of `0`, which allows leaf nodes to return a depth of `1`.

```kotlin
class Solution {

    fun maxDepth(root: TreeNode?): Int {
        if (root == null) return 0

        return maxOf(
            maxDepth(root.left),
            maxDepth(root.right),
        ) + 1
    }
}
```

### Complexity

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(h)`

Where:
- `n` is the number of nodes in the tree.
- `h` is the height of the tree.

## Step-by-Step

Let's walk through the same tree:

```text
       8
      / \
     3   12
        /  \
       9   14
```

The algorithm starts from the root node:

```text
maxDepth(8)
```

and continues until it reaches null nodes:

```text
maxDepth(8)
├── maxDepth(3)
│   ├── maxDepth(null)
│   └── maxDepth(null)
└── maxDepth(12)
    ├── maxDepth(9)
    │   ├── maxDepth(null)
    │   └── maxDepth(null)
    └── maxDepth(14)
        ├── maxDepth(null)
        └── maxDepth(null)
```

Once a `null` node is reached, the recursion starts returning results:

```text
maxDepth(3)

= max(
    maxDepth(null),
    maxDepth(null)
) + 1

= max(0, 0) + 1

= 1
```

```text
maxDepth(9)

= max(
    maxDepth(null),
    maxDepth(null)
) + 1

= max(0, 0) + 1

= 1
```

```text
maxDepth(14)

= max(
    maxDepth(null),
    maxDepth(null)
) + 1

= max(0, 0) + 1

= 1
```

Now node `12` has the depth of both children:

```text
maxDepth(12)

= max(
    maxDepth(9),
    maxDepth(14)
) + 1

= max(1, 1) + 1

= 2
```

Finally, we return to the root node:

```text
maxDepth(8)

= max(
    maxDepth(3),
    maxDepth(12)
) + 1

= max(1, 2) + 1

= 3
```

The maximum depth of the tree is:

```kotlin
Output: 3
```
