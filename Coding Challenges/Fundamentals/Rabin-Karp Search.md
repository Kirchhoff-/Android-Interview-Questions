# Rabin-Karp Search

Substring search is a common problem in computer science. The simplest approach is to compare the pattern with the text one character at a time. While easy to understand and implement, this approach often results in many unnecessary comparisons, especially when processing large texts or searching for multiple patterns.

To address this problem, Michael O. Rabin and Richard M. Karp introduced a hashing-based approach to substring search.

## Motivation

Suppose we want to find the pattern `"world"` in the following text:

```text
Hello wonderful world!
```

The naive solution checks every possible position in the text until it finds a match. At each position, it compares the pattern with the current substring character by character.

```text
Hello wonderful world!
world
↑
```

The first comparison fails immediately because `'H'` and `'w'` are different. The algorithm then shifts the pattern by one position and repeats the entire process.

```text
Hello wonderful world!
 world
 ↑
```

This continues until the pattern is found or the end of the text is reached.

Instead of comparing characters first, Rabin-Karp takes a different approach. It computes a **hash value** for both the pattern and the current substring of the same length. If the hash values are different, the algorithm immediately skips the current position because the strings cannot be equal. Only when the hash values match does it perform a character-by-character comparison to verify the result.

The obvious question is: **How can we efficiently compute a hash for every substring without recalculating it from scratch each time?** The answer is a technique called **Rolling Hash**.

## Hash Functions and Rolling Hash

A **hash function** converts a string into a numeric value called a **hash**. 

For example:

```text
Hello -> 593821

World -> 128475
```

The exact formula isn't important. What matters is that the same string always produces the same hash value.

If the hash values are different, the strings **cannot** be equal. However, if the hash values are the same, the strings **might** be equal. This situation is called a **hash collision**, which is why Rabin-Karp always performs a final character-by-character comparison before reporting a match.

The next challenge is efficiency. Suppose we are searching for a pattern of length `5` inside a text containing one million characters. Recomputing a hash from scratch for every possible substring would be too expensive. This is where the idea of **Rolling Hash** comes in. Instead of recalculating the hash for every new window from scratch, Rolling Hash updates the previous hash when the search window moves by one character.

For example:

```text
HelloWorld

Hello
 elloW
  lloWo
   loWor
```

Each new window differs from the previous one by only two characters:

- one character leaves the window;
- one new character enters the window.

Rolling Hash takes advantage of this observation. Instead of recalculating the entire hash, it removes the contribution of the outgoing character and adds the contribution of the incoming one.

As a result, updating the hash takes `O(1)` time instead of `O(n)`, where `n` is the length of the pattern. This optimization is one of the key ideas behind the efficiency of the Rabin-Karp algorithm.

## Implementation

The algorithm begins by computing the hash value of the pattern and the first substring (window) of the same length in the text. It then slides the window through the text one character at a time.

For each window:

1. Compare the hash of the pattern with the hash of the current window;
2. If the hashes are different, continue to the next window;
3. If the hashes match, perform a character-by-character comparison;
4. If all characters match, the pattern has been found.

```kotlin
fun rabinKarpSearch(text: String, pattern: String): Int {
    val patternHash = calculateHash(pattern)
    var windowHash = calculateHash(text.substring(0, pattern.length))

    for (startIndex in 0..text.length - pattern.length) {
        if (windowHash == patternHash) {
            if (pattern == text.substring(startIndex, startIndex + pattern.length)) {
                return startIndex
            }
        }

        windowHash = windowHash = updateRollingHash(windowHash, startIndex)
    }

    return -1
}
```

Suppose we want to find:

```text
Pattern: "abc"

Text: "xxabcxx"
```

| Window | Hash Match | Character Comparison |
|--------|:----------:|:--------------------:|
| `xxa` | ❌ | Skip |
| `xab` | ❌ | Skip |
| `abc` | ✅ | Match found |
| `bcx` | ❌ | Skip |
| `cxx` | ❌ | Skip |

**Complexity**

| Operation | Complexity |
|-----------|-----------:|
| Time | **O(n + m)** average |
| Time (worst case) | **O(n × m)** |
| Space | **O(1)** |

Where:

- `n` — length of the text.
- `m` — length of the pattern.

The worst-case complexity occurs when many hash collisions happen, forcing the algorithm to repeatedly perform full character comparisons.

## Pros / Cons

**Pros:**
- Simple algorithm with a clear and intuitive idea;
- Rolling Hash avoids recomputing hash values from scratch for every window;
- Average-case time complexity of `O(n + m)`.

**Cons:**
- Hash collisions require an additional character-by-character comparison;
- Choosing a good hash function is important for performance;
- Worst-case time complexity degrades to `O(n × m)`.

# Further Reading
- [Rabin–Karp Algorithm (Wikipedia)](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm)
- [Rabin-Karp Algorithm for String Matching (CP-Algorithms)](https://cp-algorithms.com/string/rabin-karp.html)
- [Rabin-Karp Algorithm for Pattern Searching (GeeksforGeeks)](https://www.geeksforgeeks.org/dsa/rabin-karp-algorithm-for-pattern-searching/)
- [Efficient Randomized Pattern-Matching Algorithms (Original Paper)](https://didawiki.cli.di.unipi.it/lib/exe/fetch.php/bio/kr87.pdf)
