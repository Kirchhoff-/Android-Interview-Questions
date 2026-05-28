# Valid Parentheses
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Stack-blue)

Given a string containing only the following characters:

```text
()[]{}
```

determine whether the string is valid.

A string is considered valid if:
- Every opening bracket has a corresponding closing bracket of the same type;
- Brackets are closed in the correct order;
- Every closing bracket has a matching opening bracket.

---

## Examples

**Example 1**

```text
Input: "()"
Output: true
```

**Example 2**

```text
Input: "()[]{}"
Output: true
```

**Example 3**

```text
Input: "(]"
Output: false
```

**Example 4**

```text
Input: "([])"
Output: true
```

**Example 5**

```text
Input: "([)]"
Output: false
```

## Solution 1: Removing Valid Pairs

The first natural idea is to use the standard Kotlin String API. This approach is based on a simple observation: every valid pair of brackets can be removed from the string.

For example:

```text
"([])" -> "()" -> ""
```

If after repeatedly removing all valid pairs the string becomes empty, then all brackets were matched correctly.

```kotlin
fun isValid(s: String): Boolean {
    if (s.length % 2 != 0) {
        return false
    }

    var current = s

    while (true) {
        val updated = current
            .replace("()", "")
            .replace("[]", "")
            .replace("{}", "")

        if (updated == current) {
            break
        }

        current = updated
    }

    return current.isEmpty()
}
```

This solution works because valid inner pairs are removed first, and after that outer pairs may become adjacent and removable too.

However, this approach is not the most efficient one. Each `replace` call creates a new string, and we may need to repeat this process multiple times. So while the solution is easy to understand, it does more work than necessary.

**Time complexity:** `O(n²)` in the worst case

**Space complexity:** `O(n)`

## Solution 2: Stack

The more appropriate way to solve this problem is to use a stack.

The idea is simple: when we meet an opening bracket, we put it into the stack. When we meet a closing bracket, we check whether the last opening bracket matches it. If it does, we continue processing the string. Otherwise, we immediately return `false` because the brackets are not balanced.

```kotlin
fun isValid(s: String): Boolean {
    if (s.length % 2 != 0) {
        return false
    }

    val queue: ArrayDeque<String> = ArrayDeque()

    for (i in 0..<s.length) {
        val element: Char = s[i]

        when (element) {
            '(' -> queue.addLast("(")
            '{' -> queue.addLast("{")
            '[' -> queue.addLast("[")
            ')' -> if (queue.removeLastOrNull() != "(") return false
            '}' -> if (queue.removeLastOrNull() != "{") return false
            ']' -> if (queue.removeLastOrNull() != "[") return false
        }
    }

    return queue.isEmpty()
}
```

This solution is more efficient because each character is processed only once. We do not need to repeatedly create new strings or scan the same input multiple times.

**Time complexity:** `O(n)`

**Space complexity:** `O(n)`

## Solution 3: Stack With Expected Closing Brackets

This solution uses the same stack idea as the previous one, but slightly simplifies the logic.

Instead of storing opening brackets in the stack, we immediately store the closing bracket that we expect to see later.

For example, when we see:

```text id="7qv1ol"
(
```

we push:

```text id="v2s1n2"
)
```

into the stack.

Then, when we meet a closing bracket, we simply compare it with the last expected value from the stack.

```kotlin id="1i0c7z"
fun isValid(s: String): Boolean {
    if (s.length % 2 != 0) {
        return false
    }

    val stack = ArrayDeque<Char>()

    for (char in s) {
        when (char) {
            '(' -> stack.addLast(')')
            '[' -> stack.addLast(']')
            '{' -> stack.addLast('}')
            else -> {
                if (stack.removeLastOrNull() != char) {
                    return false
                }
            }
        }
    }

    return stack.isEmpty()
}
```

This approach removes the need to manually compare multiple bracket pairs:

```kotlin id="pqlm0n"
')' -> ...
']' -> ...
'}' -> ...
```

and makes the solution slightly shorter and cleaner.

The overall complexity remains the same.

**Time complexity:** `O(n)`

**Space complexity:** `O(n)`

