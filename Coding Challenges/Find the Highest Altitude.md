# Find The Highest Altitude
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-One%20Pass-blue)

A biker starts the trip at altitude `0`.

The `gain` array contains altitude changes between consecutive points on the route. Positive values indicate an ascent, while negative values indicate a descent.

Return the highest altitude reached at any point during the trip.

## Constraints

```kotlin
n == gain.length

1 <= n <= 100

-100 <= gain[i] <= 100
```

## Examples

**Example 1**:

```kotlin
Input: gain = [5, 3, -2, 4]

Output: 10
```

The biker reaches the following altitudes:

```text
[0, 5, 8, 6, 10]
```

The highest altitude is `10`.

**Example 2**:

```kotlin
Input: gain = [-2, -3, 4, 2]

Output: 1
```

The biker reaches the following altitudes:

```text
[0, -2, -5, -1, 1]
```

The highest altitude is `1`.

**Example 3**:

```kotlin
Input: gain = [-4, -3, -2, -1]

Output: 0
```

The biker reaches the following altitudes:

```text
[0, -4, -7, -9, -10]
```

The highest altitude is `0`.

## Solution

The `gain` array contains altitude changes rather than the actual altitudes.

We start at altitude `0`, so we can reconstruct the biker's altitude step by step while traversing the array.

At each iteration:
1. Update the current altitude;
2. Compare it with the highest altitude seen so far;
3. Update the result if the current altitude is greater.

```kotlin
fun largestAltitude(gain: IntArray): Int {
    var largestAltitude: Int = 0
    var currentAltitude: Int = 0

    for (value in gain) {
        currentAltitude += value

        if (largestAltitude < currentAltitude) {
            largestAltitude = currentAltitude
        }
    }

    return largestAltitude
}
```

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

## Step-by-step

Let's walk through the algorithm using the following input:

```kotlin
gain = [5, 3, -2, 4]
```

The biker starts at altitude `0`.

```text
currentAltitude = 0
largestAltitude = 0
```

### Step 1

```text
value = 5
currentAltitude = 0 + 5 = 5
largestAltitude = 5
```

### Step 2

```text
value = 3
currentAltitude = 5 + 3 = 8
largestAltitude = 8
```

### Step 3

```text
value = -2
currentAltitude = 8 - 2 = 6
largestAltitude = 8
```

### Step 4

```text
value = 4
currentAltitude = 6 + 4 = 10
largestAltitude = 10
```

After processing all values, the highest altitude reached during the trip is `10`.

```text
return 10
```
