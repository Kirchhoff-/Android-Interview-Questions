# Best Time to Buy and Sell Stock
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen)
![Patterns](https://img.shields.io/badge/Pattern-One%20Pass-blue)

Given an array `prices` where `prices[i]` represents the stock price on day `i`.

You may choose exactly one day to buy a stock and one future day to sell it. Return the maximum profit you can achieve from this transaction.

If no profit is possible, return `0`.

### Constraints

* `1 <= prices.length <= 10^5`
* `0 <= prices[i] <= 10^4`

### Examples

**Example 1**:

```kotlin
Input: prices = [7, 1, 5, 3, 6, 4]
Output: 5
```

Explanation:

```text
Buy at price 1 and sell at price 6.

Day:    0  1  2  3  4  5
Price: [7, 1, 5, 3, 6, 4]
             ^        ^
            Buy      Sell
```

Profit:

```text
6 - 1 = 5
```

Therefore, we return `5`.

**Example 2**:

```kotlin
Input: prices = [7, 6, 4, 3, 1]
Output: 0
```

Explanation:

```text
The stock price decreases every day.

Day:    0  1  2  3  4
Price: [7, 6, 4, 3, 1]
```

Since there is no profitable transaction, we return `0`.

**Example 3**:

```kotlin
Input: prices = [2, 4, 1]
Output: 2
```

Explanation:

```text
Buy at price 2 and sell at price 4.

Day:    0  1  2
Price: [2, 4, 1]
         ^  ^
        Buy Sell
```

Profit:

```text
4 - 2 = 2
```

Therefore, we return `2`.

## Solution 1. Brute Force

The most straightforward solution is to check every possible pair of buy and sell days.

For each day, we can treat it as a potential buy day. Then we check every future day as a potential sell day, calculate the profit, and keep the maximum value.

```kotlin
fun maxProfit(prices: IntArray): Int {
    var maximumProfit: Int = 0

    for (buyDay in prices.indices) {
        for (sellDay in buyDay + 1 until prices.size) {
            val profit: Int = prices[sellDay] - prices[buyDay]

            if (profit > maximumProfit) {
                maximumProfit = profit
            }
        }
    }

    return maximumProfit
}
```

This solution is easy to understand, but it checks too many combinations.

For every possible buy day, we try all future sell days. Because of that, the number of operations grows very quickly as the input size increases.

**Complexity**

Time complexity: `O(n²)`

Space complexity: `O(1)`

## Solution 2. One Pass

The brute force solution checks all possible buy and sell pairs, but we do not actually need to compare every pair.

When we are looking at the current price, we only need to know two things:

* the lowest price we have seen so far;
* the best profit we have found so far.

If the current price is lower than `minimumPriceSoFar`, we update the buying price.

Otherwise, we calculate how much profit we would get if we sold at the current price and update `maximumProfitSoFar` if this profit is better.

```kotlin
fun maxProfit(prices: IntArray): Int {
    var minimumPriceSoFar: Int = Int.MAX_VALUE
    var maximumProfitSoFar: Int = 0

    for (price in prices) {
        if (price < minimumPriceSoFar) {
            minimumPriceSoFar = price
        }

        val potentialProfit: Int = price - minimumPriceSoFar

        if (potentialProfit > maximumProfitSoFar) {
            maximumProfitSoFar = potentialProfit
        }
    }

    return maximumProfitSoFar
}
```

This solution makes only one pass through the array.

It works because at each day we already know the best price we could have used to buy the stock before or on that day. Therefore, we can immediately calculate the best profit for selling at the current price.

**Complexity**

Time complexity: `O(n)`

Space complexity: `O(1)`

## Step-by-Step Example

Let's use the following input:

```kotlin
prices = [7, 1, 5, 3, 6, 4]
```

We start with:

```text
minimumPriceSoFar = 7
maximumProfitSoFar = 0
```

### Day 0

```text
price = 7
minimumPriceSoFar = 7
potentialProfit = 7 - 7 = 0
maximumProfitSoFar = 0
```

### Day 1

```text
price = 1
```

Since `1 < 7`, we update the minimum price:

```text
minimumPriceSoFar = 1
potentialProfit = 1 - 1 = 0
maximumProfitSoFar = 0
```

### Day 2

```text
price = 5
minimumPriceSoFar = 1
potentialProfit = 5 - 1 = 4
maximumProfitSoFar = 4
```

### Day 3

```text
price = 3
minimumPriceSoFar = 1
potentialProfit = 3 - 1 = 2
maximumProfitSoFar = 4
```

### Day 4

```text
price = 6
minimumPriceSoFar = 1
potentialProfit = 6 - 1 = 5
maximumProfitSoFar = 5
```

### Day 5

```text
price = 4
minimumPriceSoFar = 1
potentialProfit = 4 - 1 = 3
maximumProfitSoFar = 5
```

The best profit we found is:

```text
maximumProfitSoFar = 5
```

Therefore, we return `5`.

