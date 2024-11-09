# Minimum Array End

## Problem Overview

[Problem Link](https://leetcode.com/problems/minimum-array-end/description/?envType=daily-question&envId=2024-11-09)

You are given an integer `n` and an integer `x`. The task is to construct an array `nums` of size `n` such that:

1. **The bitwise AND** of all elements in the array is equal to `x`.
2. **The last element (`nums[n - 1]`)** is the smallest possible element.

Return the minimum possible value of `nums[n - 1]`.

### Key Observations

1. **AND Property**: To ensure the AND of all elements equals `x`, every element must contain at least the bits set in `x`. Specifically:

   - If a bit is `1` in `x`, it must be `1` in all elements of `nums`.
   - If a bit is `0` in `x`, the bit can be `0` or `1` in each element of `nums`.

2. **Constructing the Array**:
   - The first element `nums[0]` can be initialized to `x`.
   - For the other `n-1` elements, we want to set their free bits (bits where `x` is `0`) using bits from `n` to maximize the value while ensuring the AND condition is met.
   - The goal is to minimize the last element (`nums[n-1]`).

### Solution Strategy

1. **Start with `x`**: Set the first element to `x`, as we need to maintain the required set bits from `x` in all elements.
2. **Set the Free Bits**: For the remaining elements, iterate over the free bit positions (those where `x` has `0`), and set them to `1` where possible to maximize the values while maintaining the AND condition.
3. **Minimize the Last Element**: The last element (`nums[n-1]`) should be the smallest possible value while satisfying the AND condition.

### Solution Code

```cpp
class Solution {
public:
    long long minEnd(int n, int x) {
        long long ans = (long long)x;  // Start with the value of x
        n -= 1;  // Since we already use x as the first element, we start with n-1 elements to modify

        // Iterate over the 64 bits (to account for any large values of n or x)
        for (int i = 0; i < 64 && n > 0; i++) {
            // Check if the i-th bit of ans is 0
            if ((ans >> i & 1) == 0) {
                // Take the lowest bit from n and set it in ans
                ans |= (long long)(n & 1) << i;
                // Shift n to the right to use the next bit
                n >>= 1;
            }
        }

        return ans;  // Return the minimum possible value of nums[n - 1]
    }
};
```

### Explanation of Key Code Elements

- **`ans = (long long)x`**: The initial element `ans` is set to `x` because we need to maintain the set bits in all elements.
- **`n -= 1`**: Decrement `n` because the first element `nums[0]` is already set to `x`.
- **`(ans >> i & 1) == 0`**: Checks if the `i`-th bit of `ans` is `0`.
- **`ans |= (long long)(n & 1) << i`**: Sets the free bit of `ans` using the lowest bit of `n`.
- **`n >>= 1`**: Shifts `n` to the right to use the next bit in the next iteration.

### Example Walkthrough

#### Input:

```
n = 2, x = 7
```

#### Output:

```
15
```

#### Explanation:

We need to form an array `nums` of size 2 such that the bitwise AND of all elements is equal to `x = 7`. The smallest possible value for the last element is `15`.

The array can be `[7, 15]` where:

- The AND of `7` (`0111` in binary) and `15` (`1111` in binary) is `7`, satisfying the AND condition.
- The smallest possible last element (`nums[n-1]`) is `15`.

### Complexity Analysis

- **Time Complexity**: $$O(Log(n))$$, because we iterate over the bits of $$n$$ (which is proportional to $$OLog(n)$$.
- **Space Complexity**: $$O(1)$$, since we only use a constant amount of extra space.
