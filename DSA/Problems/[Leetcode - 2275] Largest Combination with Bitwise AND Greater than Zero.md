# [Leetcode] Largest Combination with Bitwise AND Greater than Zero

## Problem Overview

[Problem Link](https://leetcode.com/problems/largest-combination-with-bitwise-and-greater-than-zero/description/)

Given a list of 24-bit integers (`candidates`), find the size of the largest combination where the bitwise AND of all numbers in the combination is greater than zero.

### Key Observations

- For a bitwise AND to be greater than zero, at least one specific bit position must have a `1` in each number in the combination.
- Since candidates are 24 bits long, we can analyze each bit position (from the 0th to the 23rd) independently.
- For each bit position, count how many numbers have a `1` in that position. The maximum count across all bit positions gives the size of the largest valid combination.

## New Info

- `int bit = (num >> n) & 1` can be used to find the nth bit of an integer.

### Solution Explanation

1. Loop over each of the 24 bits (from 23 to 0).
2. For each bit position, count the numbers in `candidates` that have a `1` at that bit position.
3. Track the highest count across all bit positions as the answer.

### Solution Code

```cpp
class Solution {
public:
    int largestCombination(vector<int>& candidates) {
        int max_combination_size = 1;

        for (int i = 23; i >= 0; --i) {
            int count = 0;

            for (int num : candidates) {
                // Check if the ith bit is set (1) in the current number
                if ((num >> i) & 1) {
                    count++;
                }
            }

            // Update max_combination_size if a larger combination is found
            max_combination_size = max(max_combination_size, count);
        }

        return max_combination_size;
    }
};
```

### Explanation of Key Code Elements

- `int bit = (num >> i) & 1;`: Shifts `num` right by `i` bits, then uses `& 1` to isolate the `i`th bit.
- `max(max_combination_size, count);`: Ensures we store the largest count found across all bit positions.
