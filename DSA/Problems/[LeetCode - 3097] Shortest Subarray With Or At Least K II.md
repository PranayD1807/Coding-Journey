# [LeetCode] Shortest Subarray With Or At Least K II

### Problem Overview

**[Problem Link](https://leetcode.com/problems/shortest-subarray-with-or-at-least-k-ii/)**

Given an integer array `nums` and an integer `k`, find the length of the shortest subarray with a bitwise OR that is at least `k`. If no such subarray exists, return -1.

### Key Observations

1. **Bitwise OR Properties**: The bitwise OR operation allows us to accumulate bits across a subarray. To find a minimum subarray length, we can maintain a sliding window with an accumulating OR, adjusting the window size to meet or exceed `k`.
2. **Bit Tracking with an Array (`store`)**: We use a `store` array of size 32 (for 32 bits) to track the count of `1`s at each bit position in the current window. This allows efficient OR calculation for the current subarray.
3. **Incremental OR Calculation**: Instead of recomputing the OR for each window from scratch, we track changes in the OR value by updating bit counts in `store`. This helps determine if adding or removing an element achieves the target OR.

### Solution Explanation

1. **Initialize Variables**:
   - `store`: A 32-element array where each entry tracks the number of `1`s at that bit position in the current window.
   - `globalXor`: The cumulative XOR of all elements.
   - `ans`: Stores the minimum subarray length found, initialized to `INT_MAX` for comparison.
2. **Define Helper Functions**:

   - **`addNum(nums, i)`**: Adds the bits of `nums[i]` to `store`, updating each bit position as necessary.
   - **`getVal()`**: Calculates the current OR value of the window by evaluating bits in `store`. If a position in `store` is non-zero, it means that bit is `1` in the OR result.
   - **`remove(nums, i)`**: Removes the bits of `nums[i]` from `store`, used when shrinking the window.

3. **Sliding Window**:
   - Using two pointers, `s` (start) and `e` (end), the algorithm maintains a window that expands with `e` and contracts with `s` until the OR result of the window is at least `k`.
   - If the OR of the window meets or exceeds `k`, we update `ans` to store the smallest subarray length found.
   - Continue expanding `e` or contracting `s` based on whether the OR meets the requirement.
4. **Return Result**: If a valid subarray was found, return `ans`, otherwise return -1.

### Solution Code

```cpp
class Solution {
private:
    vector<int> store;

    // Adds the bit representation of nums[i] to the `store` array
    void addNum(vector<int>& nums, int i) {
        if (i >= nums.size()) return;
        int num = nums[i];
        for (int j = 0; j < 32; j++) {
            int bit = (num >> j) & 1;
            store[j] += bit;
        }
    }

    // Calculates the current bitwise OR of all elements in the window by decoding `store`
    int getVal() {
        int res = 0;
        for (int i = 0; i < 32; i++) {
            if (store[i] != 0) {
                res |= (1 << i);
            }
        }
        return res;
    }

    // Removes the bit representation of nums[i] from the `store` array
    void remove(vector<int>& nums, int i) {
        if (i >= nums.size()) return;
        int num = nums[i];
        for (int j = 0; j < 32; j++) {
            int bit = (num >> j) & 1;
            store[j] -= bit;
        }
    }

public:
    int minimumSubarrayLength(vector<int>& nums, int k) {
        int s = 0, e = 0;
        int ans = INT_MAX;
        store.resize(32, 0);  // Initialize bit tracking array

        addNum(nums, 0);

        // Sliding window approach
        while (e < nums.size() && s < nums.size()) {
           if (getVal() < k) {
                addNum(nums, ++e);
            } else {
                ans = min(ans, e - s + 1);
                s == e ? addNum(nums, ++e) : remove(nums, s++);
            }
        }
       return ans == INT_MAX ? -1 : ans;
    }
};
```

### Explanation of Key Code Elements

- **`addNum`**: Tracks bits for each element in the sliding window by updating `store`.
- **`getVal`**: Decodes `store` to determine the OR result for the current window.
- **Sliding Window Logic**: Manages the expansion and contraction of the window based on whether the OR requirement is met.

### Complexity Analysis

- **Time Complexity**: $$O(n \times 32) = O(n)$$, where $$n$$ is the size of `nums` since the number of bits is fixed.
- **Space Complexity**: $$O(32) = O(1)$$, for `store`.
