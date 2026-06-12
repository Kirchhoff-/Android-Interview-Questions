# First Bad Version
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-Binary%20Search-blue)

A product consists of versions numbered from `1` to `n`.

Once a bad version is introduced, every version released after it is also bad.

You are given an API `isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version while minimizing the number of API calls.

### Constraints

```kotlin
1 <= n <= 2^31 - 1
```

### Examples

**Example 1**:
```kotlin
Input: n = 8, bad = 5
Output: 5
```

Explanation:

```text
Version 5 is the first bad version.

Call isBadVersion(1) -> false
Call isBadVersion(2) -> false
Call isBadVersion(3) -> false
Call isBadVersion(4) -> false
Call isBadVersion(5) -> true
```

Therefore, version `5` is the first bad version.

**Example 2**:

```kotlin
Input: n = 10, bad = 7
Output: 7
```

Explanation:

```text
Version 7 is the first bad version.

Call isBadVersion(1) -> false
Call isBadVersion(2) -> false
Call isBadVersion(3) -> false
Call isBadVersion(4) -> false
Call isBadVersion(5) -> false
Call isBadVersion(6) -> false
Call isBadVersion(7) -> true
```

Therefore, version `7` is the first bad version.

## Solution
This problem is a logical continuation of the [Sqrt(x)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Sqrt(x).md) challenge.

If you are new to Binary Search and would like a detailed explanation of the algorithm, I recommend reviewing the [Sqrt(x)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Sqrt(x).md) challenge first. In this article, we will focus only on the concepts that are unique to this problem.

----

The main difference is that we do not have an array in this problem. Instead, we are given the following API:

```kotlin
isBadVersion(version)
```

At first glance this may seem like a completely different problem, but in reality very little has changed. The array itself is not what makes Binary Search work. What matters is that we still have a search range and a condition that changes only once.

If we represent all versions as boolean values, the problem becomes much easier to understand:

```text
false false false true true true
```

Just like in [Sqrt(x)](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Coding%20Challenges/Sqrt(x).md), our goal is to find the transition point. In this case, we need to find and return the first `true`.

The rest of the algorithm remains the same. On each iteration, we check the middle version and discard half of the remaining search space. If `isBadVersion(version)` returns `true`, we move the right boundary. Otherwise, we move the left boundary. We continue shrinking the range until only one possible bad version remains.

```kotlin
fun firstBadVersion(n: Int): Int {
    if (n == 1) return n

    var rangeStart = 0
    var rangeEnd = n

    while (true) {
        val newCandidateVersion = rangeStart + (rangeEnd - rangeStart) / 2

        if (isBadVersion(newCandidateVersion)) {
            rangeEnd = newCandidateVersion
        } else {
            rangeStart = newCandidateVersion
        }

        if (rangeEnd - rangeStart == 1) {
            return rangeEnd
        }
    }
}
```

**Complexity**

Time complexity: `O(log n)`

Space complexity: `O(1)`

## Explanation

We start with the following search range:

```text
0 ... n
```

`rangeStart` represents the last known good version, while `rangeEnd` represents the first possible bad version.

On each iteration, we calculate the middle version:

```kotlin
val newCandidateVersion = rangeStart + (rangeEnd - rangeStart) / 2
```

At first glance, it may seem much simpler to calculate the middle version like this:

```kotlin
(rangeStart + rangeEnd) / 2
```

However, this approach can cause an integer overflow when working with very large numbers.

The idea behind the actual formula is simple. We first calculate the size of the current search range:

```text
rangeEnd - rangeStart
```

Then take half of that range:

```text
(rangeEnd - rangeStart) / 2
```

And finally move that distance from the left boundary:

```text
rangeStart + ...
```

For example:

```text
rangeStart = 10
rangeEnd = 20

Current range = 10
Half of the range = 5

10 + 5 = 15
```

As a result, `15` becomes the middle version.

Once we have the candidate version, we check whether it is bad:

```kotlin
if (isBadVersion(newCandidateVersion))
```

If the result is `true`, then the first bad version can be either `newCandidateVersion` itself or some version before it. In this case, we move the right boundary:

```kotlin
rangeEnd = newCandidateVersion
```

Otherwise, the current version is good, meaning the first bad version must be located after it:

```kotlin
rangeStart = newCandidateVersion
```

We continue shrinking the search range until we find the transition point:

```text
false false false | true true true
```

At this point, `rangeStart` points to the last known good version and `rangeEnd` points to the first bad version. Therefore, we can safely return `rangeEnd`.
