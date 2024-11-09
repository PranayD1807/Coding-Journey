# [Leetcode] Maximum XOR for Each Query

## Problem Overview

[Problem Link](https://leetcode.com/problems/maximum-xor-for-each-query/)

You are given an integer array `nums` and an integer `maximumBit`. For each query, find the maximum XOR of the cumulative XOR of all elements in `nums` with an integer that has at most `maximumBit` bits. Return the results in reverse order of the queries.

### Key Observations

- **Cumulative XOR**: The XOR operation has cumulative properties, which allows us to compute a running XOR (`globalXor`) for the entire array as we iterate over it. This running XOR will be used in each query to find the maximum possible XOR result.
- **Mask Calculation**: To find the maximum XOR of `globalXor` with a number of at most `maximumBit` bits, we can create a mask with `maximumBit` bits all set to `1`. This mask is computed as `(1 << maximumBit) - 1`.
- **Maximum XOR Result**: For each query, the maximum XOR result can be found by XOR-ing `globalXor` with the mask. This operation flips the bits of `globalXor` up to `maximumBit` to achieve the highest possible XOR result.
- **Reverse Order Requirement**: Since the queries are supposed to be answered in reverse order, we store the results as we iterate and finally reverse them before returning.

### Solution Explanation

1. **Initialization**: Set up `globalXor` as `0` and create a mask with `maximumBit` bits set to `1` to use throughout the loop.
2. **Iterate through `nums`**:
   - For each element in `nums`, update `globalXor` by XOR-ing it with the current element.
   - Use `globalXor ^ mask` to find the maximum XOR result for the current cumulative XOR up to this point.
   - Store this result in a vector `ans`.
3. **Reverse Order**: Since results need to be returned in reverse, store each result in the vector in reverse order.
4. **Return the Result**: Return the `ans` vector.

### Example

Let’s say:

- `maximumBit = 3`, so the mask will use the first 3 bits.
- `nums = [2, 3, 4]`

The steps go as follows:

1. **Create the Bitmask**:

   - `mask = (1 << maximumBit) - 1`
   - `mask = (1 << 3) - 1 = 8 - 1 = 7`
   - Binary of `7` is `0000 0111`, so the mask has the lowest 3 bits set to `1`.

2. **Calculate Result Using XOR and Mask**:

   - Initialize `globalXor = 0`.

   - **Iteration 1** (for `nums[0] = 2`):

     - `globalXor ^= 2` → `globalXor = 0 ^ 2 = 2` (binary: `0000 0010`)
     - `result = globalXor ^ mask = 2 ^ 7 = 5` (binary: `0000 0101`)

   - **Iteration 2** (for `nums[1] = 3`):

     - `globalXor ^= 3` → `globalXor = 2 ^ 3 = 1` (binary: `0000 0001`)
     - `result = globalXor ^ mask = 1 ^ 7 = 6` (binary: `0000 0110`)

   - **Iteration 3** (for `nums[2] = 4`):
     - `globalXor ^= 4` → `globalXor = 1 ^ 4 = 5` (binary: `0000 0101`)
     - `result = globalXor ^ mask = 5 ^ 7 = 2` (binary: `0000 0010`)

After calculating each `result`, they are stored in reverse order for the final answer.

### Solution Code

```cpp
class Solution {
public:
    vector<int> getMaximumXor(vector<int>& nums, int maximumBit) {
        int mask = (1 << maximumBit) - 1;  // Create a mask for maximum XOR with `maximumBit` bits
        int globalXor = 0;
        int n = nums.size();
        vector<int> ans(n);  // Pre-allocate the vector for better performance

        // Compute the cumulative XOR and maximum XOR for each query
        for(int i = 0; i < n; i++) {
            globalXor ^= nums[i];
            ans[n - i - 1] = globalXor ^ mask;  // Store result in reverse order
        }

        return ans;
    }
};
```

### Explanation of Key Code Elements

- **`mask = (1 << maximumBit) - 1`**: Creates a mask where only the lowest `maximumBit` bits are `1`. This ensures that the XOR result is limited to numbers with at most `maximumBit` bits.
- **`globalXor ^= nums[i]`**: Maintains a running cumulative XOR of all elements in `nums` up to the current element, `i`.
- **`globalXor ^ mask`**: Finds the maximum possible XOR by flipping `globalXor`'s bits up to `maximumBit` positions.
- **`ans[n - i - 1]`**: Stores results directly in reverse order, avoiding the need for a final reverse operation.

### Complexity Analysis

- **Time Complexity**: $$O(n)$$, where $$n$$ is the size of `nums`, since we iterate through `nums` once.
- **Space Complexity**: $$O(n)$$, for storing the result in the `ans` vector.
