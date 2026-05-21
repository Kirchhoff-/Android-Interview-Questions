# Power of Two
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Math-blue)
![Pattern](https://img.shields.io/badge/Pattern-Bit%20Manipulation-blue)

Given an integer `n`, determine whether it is a power of two. A number is a power of two if it can be written as `2^x`, where `x >= 0`.

Examples:
- `1` → `true` (`2⁰`)
- `16` → `true` (`2⁴`)
- `3` → `false`

## Solution 1: Repeated Division

The most straightforward approach is to keep dividing the number by `2`.

**Why does this work?**

The key idea is simple: a power of two is always divisible by `2` without a remainder (until we reach `1`).

Examples:
- `16 % 2 = 0` → divide → `8`
- `8 % 2 = 0` → divide → `4`
- `4 % 2 = 0` → divide → `2`
- `2 % 2 = 0` → divide → `1`

If we successfully reach `1`, the number is a power of two. But If at any point dividing by `2` leaves a non-zero remainder, the number is not a power of two.

Examples:
- `12 % 2 = 0` → `6`
- `6 % 2 = 0` → `3`
- `3 % 2 = 1` ❌

We also need to handle edge cases:
- 0 is not a power of two
- negative numbers are not powers of two

```
fun isPowerOfTwo(n: Int): Boolean {
    if (n <= 0) return false

    var current = n

    while (current % 2 == 0) {
        current /= 2
    }

    return current == 1
}
```

**Time complexity**: `O(log n)`

**Space complexity**: `O(1)`

## Solution 2: Kotlin Standard Library Approach

This is probably my favorite solution in terms of code readability and simplicity.

The idea is simple:
- Convert the number to its binary representation
- Count how many `1`s it contains
- A power of two always has exactly one `1` in binary form

Examples:

- `1` → `1`
- `2` → `10`
- `4` → `100`
- `8` → `1000`

All of them contain exactly one `1`.

But:

- `3` → `11`
- `6` → `110`
- `12` → `1100`

These contain more than one `1`, so they are not powers of two.

```
fun isPowerOfTwo(n: Int): Boolean {
    if (n <= 0) return false

    return n
        .toString(2)
        .count { it == '1' } == 1
}
```

**Time complexity**: `O(log n)`

**Space complexity**: `O(log n)`

## Solution 3: Bit Manipulation Trick

This is the most optimal solution — and the one interviewers usually expect.

As we already know from the previous solution, a power of two always contains exactly one `1` in its binary representation. 

Now here comes the trick. 

If we subtract `1`, that single `1` becomes `0`, and all bits to the right become `1`.

Example with `8`:

```
8      = 1000
8 - 1  = 0111
```

Now apply bitwise AND:

```
1000
0111
----
0000
```

Result = `0`

Now compare that with a non-power of two:

```
12     = 1100
12 - 1 = 1011
```

Bitwise AND:

```text
1100
1011
----
1000
```

Result ≠ `0`

So the final solution becomes:

```
fun isPowerOfTwo(n: Int): Boolean {
    return n > 0 && (n and (n - 1)) == 0
}
```

**Time complexity:** `O(1)`  

**Space complexity:** `O(1)`
