# Sqrt(x)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Binary%20Search-blue)

Given a non-negative integer `x`, return the square root of `x` rounded down to the nearest integer.

You must not use any built-in exponent function or operator.

### Constraints

```kotlin
0 <= x <= 2^31 - 1
```

### Examples

```kotlin
Input: x = 4
Output: 2
```

```kotlin
Input: x = 8
Output: 2
```

Explanation:

```text
√8 ≈ 2.82
```

Since we need to round the result down, the answer is `2`.

```kotlin
Input: x = 15
Output: 3
```

Explanation:

```text
√15 ≈ 3.87
```

Since we need to round the result down, the answer is `3`.

## Solution

This is one of those tasks where it is important to understand what we are actually looking for before jumping into the implementation.

On paper, we can solve this problem using a simple iteration. This is the most straightforward and obvious approach. For example, let's find the answer for `20`:

```text
1 * 1 = 1
2 * 2 = 4
3 * 3 = 9
4 * 4 = 16
5 * 5 = 25
```

We are looking for two consecutive numbers where the square of the first number is less than or equal to `x`, and the square of the second number is greater than `x`.

```text
4 * 4 = 16 <= 20
5 * 5 = 25 > 20
```

Once we find such a pair, the smaller number is the answer because the problem asks us to round the result down. For `20`, the answer is `4`.

However, multiplying numbers one by one is clearly not a good approach. We need a better way to find this pair.

Since we are looking for two consecutive numbers that form a boundary, let's look at the initial range from `0` to `20`. We know for sure that the answer is somewhere inside this range. Our goal is to gradually shrink the range until we find the required boundary.

To shrink the range, we repeat the following steps:
- Calculate the middle value - `(rangeStart + rangeEnd) / 2`;
- Calculate the square of the middle value;
- If the square is greater than `x`, move the right boundary to the middle value;
- If the square is less than or equal to `x`, move the left boundary to the middle value;
- Repeat until only two consecutive numbers remain in the range.

Let's see how this works for `20`:

```text
Initial range: 0..20

middle = (0 + 20) / 2 = 10
10 * 10 = 100 > 20
New range: 0..10

middle = (0 + 10) / 2 = 5
5 * 5 = 25 > 20
New range: 0..5

middle = (0 + 5) / 2 = 2
2 * 2 = 4 <= 20
New range: 2..5

middle = (2 + 5) / 2 = 3
3 * 3 = 9 <= 20
New range: 3..5

middle = (3 + 5) / 2 = 4
4 * 4 = 16 <= 20
New range: 4..5
```

Now the range contains two consecutive numbers: `4` and `5`. So the answer is `4`.

Let's look at one more example for `37`:

```text
Initial range: 0..37

middle = (0 + 37) / 2 = 18
18 * 18 = 324 > 37
New range: 0..18

middle = (0 + 18) / 2 = 9
9 * 9 = 81 > 37
New range: 0..9

middle = (0 + 9) / 2 = 4
4 * 4 = 16 <= 37
New range: 4..9

middle = (4 + 9) / 2 = 6
6 * 6 = 36 <= 37
New range: 6..9

middle = (6 + 9) / 2 = 7
7 * 7 = 49 > 37
New range: 6..7
```

Now the range contains two consecutive numbers: `6` and `7`. So the answer is `6`.

The code for this approach could look like this:
```kotlin
fun mySqrt(x: Int): Int {
    var rangeStart = 0
    var rangeEnd = x

    while (true) {
        val newPoint = (rangeStart + rangeEnd) / 2
        val squareOfNewPoint = newPoint * newPoint

        if (squareOfNewPoint > x) {
            rangeEnd = newPoint
        } else {
            rangeStart = newPoint
        }

        if (rangeEnd - rangeStart == 1) {
            return rangeStart
        }
    }
}
```

In general, this is the whole mechanism we need for this task. However, there are two important details.

First, we already know the answer for values from `0` to `4`, so we can return the result immediately and avoid running the full search:

```kotlin
if (x < 2) return x
if (x <= 4) return x / 2
```

Second, there is a more important problem. The function can receive `Int.MAX_VALUE` as input, which means that calculating:
```text
newPoint * newPoint
```

will immediately cause an integer overflow.

We can rewrite the comparison without changing its meaning:

```text
newPoint * newPoint > x
```

Divide both sides by `newPoint`:

```text
newPoint > x / newPoint
```

Now we can perform the same check without multiplying large numbers together, which completely avoids the overflow problem.

The final solution looks like this:
```kotlin
fun mySqrt(x: Int): Int {
    if (x < 2) return x
    if (x <= 4) return x / 2

    var rangeStart = 0
    var rangeEnd = x

    while (true) {
        val newPoint = (rangeStart + rangeEnd) / 2
        val threshold = x / newPoint

        if (newPoint > threshold) {
            rangeEnd = newPoint
        } else {
            rangeStart = newPoint
        }

        if (rangeEnd - rangeStart == 1) {
            return rangeStart
        }
    }
}
```

**Complexity**

Time complexity: `O(log n)`

Space complexity: `O(1)`
