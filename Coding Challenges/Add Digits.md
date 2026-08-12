# Add Digits
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)

You are given a non-negative integer `num`. Calculate the sum of its digits, and if the result contains more than one digit, repeat the same process. Continue until only a single digit remains and return it.

## Constraints

* `0 <= num <= 2³¹ - 1`

## Examples

**Example 1**

```text
Input:
num = 57

Output:
3

Explanation:
5 + 7 = 12
1 + 2 = 3
```

**Example 2**

```text
Input:
num = 2468

Output:
2

Explanation:
2 + 4 + 6 + 8 = 20
2 + 0 = 2
```

**Example 3**

```text
Input:
num = 7

Output:
7

Explanation:
The number already contains only one digit, so no additional steps are required.
```

## Solution 1. String Conversion

The simplest approach is to follow the problem description directly. We can convert the number to a `String`, calculate the sum of its digits, and repeat the process until only one digit remains.

This is a simple and readable solution that could easily be used in production code when maximum performance is not required.

```kotlin
fun addDigits(num: Int): Int {
    var result = num.toString()

    while (result.length != 1) {
        val sum = result.sumOf { it.digitToInt() }
        result = sum.toString()
    }

    return result.toInt()
}
```

### Complexity

* Time complexity: `O(log n)`

The number of digits in `num` is proportional to `log n`, and each digit is processed to calculate the sum.

* Space complexity: `O(log n)`

The number is converted to a string, which requires space proportional to the number of digits.

## Solution 2. Arithmetic

The second solution follows the same idea as the first one, but instead of converting the number to a `String`, we work with its digits directly using arithmetic operations.

We can get the last digit of a number using the remainder operator `% 10`. For example, `1234 % 10` gives us `4`. After processing this digit, integer division by `10` removes it: `1234 / 10` gives us `123`.

By repeating these two operations, we can process all digits of the number:

```text
1234 -> digit = 4, leftover = 123
123  -> digit = 3, leftover = 12
12   -> digit = 2, leftover = 1
1    -> digit = 1, leftover = 0
```

We calculate the sum of these digits and, if the result still contains more than one digit, repeat the same process.

```kotlin
fun addDigits(num: Int): Int {
    var result = 0
    var leftover = num

    while (true) {
        while (leftover > 0) {
            val digit = leftover % 10
            leftover /= 10

            result += digit
        }

        if (result < 10) {
            break
        } else {
            leftover = result
            result = 0
        }
    }

    return result
}
```

### Complexity

* Time complexity: `O(log n)`

We process every digit of the original number, while each following sum contains significantly fewer digits.

* Space complexity: `O(1)`

We only use a constant number of integer variables and do not create any additional data structures.

### Step-by-step

Let's use `2468` as an example.

| Step | `leftover` before | `digit = leftover % 10` | `leftover /= 10` | `result` |
| ---- | ----------------: | ----------------------: | ---------------: | -------: |
| 1    |              2468 |                       8 |              246 |        8 |
| 2    |               246 |                       6 |               24 |       14 |
| 3    |                24 |                       4 |                2 |       18 |
| 4    |                 2 |                       2 |                0 |       20 |

After the first pass, `result = 20`, which still contains more than one digit. We assign it to `leftover`, reset `result` to `0`, and repeat the same process.

| Step | `leftover` before | `digit = leftover % 10` | `leftover /= 10` | `result` |
| ---- | ----------------: | ----------------------: | ---------------: | -------: |
| 1    |                20 |                       0 |                2 |        0 |
| 2    |                 2 |                       2 |                0 |        2 |

Now `result = 2`, so the process is complete.

## Solution 3. Magic

The previous solutions still require us to process the digits of the number. But there is a mathematical property that allows us to find the result in constant time, without any loops or recursion.

Naturally, discovering this approach during an interview without knowing the property beforehand is extremely difficult. This is one of those solutions that is much easier to understand and remember than to derive from scratch under interview pressure.

The key observation is that repeatedly summing the digits does not change the remainder after division by `9`.

Let's take `38` as an example. We can represent it as three tens and eight:

```text
38 = 10 + 10 + 10 + 8
```

Each `10` can also be represented as `9 + 1`:

```text
38 = (9 + 1) + (9 + 1) + (9 + 1) + 8
```

If we are interested in the remainder after division by `9`, all complete groups of `9` can be ignored. After removing them, we are left with:

```text
1 + 1 + 1 + 8
=
3 + 8
=
11
```

This is exactly the same result we get after adding the digits of `38`. In other words, adding the digits effectively removes some complete groups of `9` but keeps the same remainder:

```text
38 % 9 = 2
11 % 9 = 2
```

The same thing happens every time we repeat the process:

```text
38 -> 3 + 8 -> 11 -> 1 + 1 -> 2
```

The number becomes smaller, but its remainder after division by `9` stays the same. This means that instead of repeatedly calculating the sum of the digits, we can get the final result directly using `num % 9`.

There are only two special cases. If `num` is `0`, the result is also `0`. If a positive number is divisible by `9`, `% 9` gives us `0`, while the expected single-digit result is `9`.

```kotlin
fun addDigits(num: Int): Int {
    val remainder = num % 9

    return when {
        num == 0 -> 0
        remainder == 0 -> 9
        else -> remainder
    }
}
```

### Complexity

* Time complexity: `O(1)`

The result is calculated using a constant number of arithmetic operations regardless of the size of `num`.

* Space complexity: `O(1)`

We only use one additional integer variable.
