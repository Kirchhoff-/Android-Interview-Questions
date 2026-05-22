# Plus One
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Pattern](https://img.shields.io/badge/Pattern-Array-blue)

Given an array where each element represents a single digit of a non-negative integer, increment the number by one and return the result in the same array format.

Examples:

- `[1, 2, 3]` → `[1, 2, 4]`
- `[4, 3, 2, 1]` → `[4, 3, 2, 2]`
- `[9]` → `[1, 0]`

The main challenge here is handling carry propagation correctly.

For example:

- `129 + 1 = 130`
- `999 + 1 = 1000`

This means the change may affect multiple digits, and in some cases the resulting array may be larger than the original one.

Since the input can be large, converting the entire array into standard numeric types is not always a safe approach.

## Solution 1: Standard Kotlin API

My first instinctive solution was to treat the input as a normal number and rely on the standard Kotlin API.

The idea is straightforward:
- Convert the array into a string
- Parse it as a number
- Add one
- Convert the result back into an array of digits

```
fun plusOne(digits: IntArray): IntArray =
  digits
    .joinToString("")
    .toInt()
    .inc()
    .toString()
    .toCharArray()
    .map { it.digitToInt() }
    .toIntArray()
```

**Time Complexity:** `O(n)`  
**Space Complexity:** `O(n)`

Where:
- `O(n)` for joining digits into a string
- `O(n)` for converting the result back into an array
- additional space is required for intermediate strings and collections

This solution is concise, readable, and honestly a very natural first attempt. However, it has a serious limitation: integer overflow.

The problem allows arrays up to 100 digits long, while `Int` can only store values up to `2,147,483,647`. That means this solution works only for relatively small inputs and does not satisfy the actual problem constraints.

## Solution 2: `BigInteger`

A natural follow-up improvement is replacing `Int` with `BigInteger`. This removes the overflow limitation and allows us to work with arbitrarily large numbers.

```
fun plusOne(digits: IntArray): IntArray =
  digits
    .joinToString("")
    .toBigInteger()
    .plus(BigInteger.ONE)
    .toString()
    .map { it.digitToInt() }
    .toIntArray()
```

**Time Complexity:** `O(n)`  
**Space Complexity:** `O(n)`

This approach solves the limitations of the previous solution and works correctly for the actual problem constraints.

In regular development, this would likely be a completely reasonable solution: simple, reliable, and easy to maintain.

However, if the goal is maximum efficiency, the better approach is to avoid conversions entirely and operate directly on the input array.

## Solution 3: Carry Propagation

The most efficient approach is to work directly with the input array.

We start from the last digit and handle the carry manually:

- if the current digit is `9`, we replace it with `0` and move to the previous digit
- otherwise, we increment the current digit and return the array immediately

```
fun plusOne(digits: IntArray): IntArray {
  var i = digits.size - 1

  while (i >= 0) {
    if (digits[i] == 9) {
      digits[i] = 0
    } else {
      digits[i]++
      return digits
    }

    i--
  }

  return intArrayOf(1, *digits)
}
```

**Time Complexity:** `O(n)`  
**Space Complexity:** `O(1)` in the common case, `O(n)` when a new array is required.

If we exit the loop, it means that all digits were `9`.

For example:

- `[9]` becomes `[1, 0]`
- `[9, 9, 9]` becomes `[1, 0, 0, 0]`

In this case, we need to create a new array with `1` at the beginning and all remaining digits set to `0`.
